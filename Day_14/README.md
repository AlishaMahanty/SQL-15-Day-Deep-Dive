# Day 14 - Running Totals & Moving Averages

## 📌 Problem:

Your manager wants two things next to monthly sales:

- A running total, where each month shows the year-to-date sales up to and including that month.
- A 3-month moving average alongside it.

A regular `SUM(sales) OVER ()` gives the same grand total on every row. But a running total needs to grow month by month, while a moving average needs to slide across a fixed number of months.

### ❓ The question is:

What single addition inside `OVER()` turns a flat total into a cumulative, running total?

**Bonus Question:**

`SUM(sales) OVER (ORDER BY month)` vs `SUM(sales) OVER ()` — why does simply adding `ORDER BY` change the whole behavior?

## 💡 Solution:

Adding `ORDER BY` inside the window makes the total running instead of flat.
```
SELECT month, sales,
    SUM(sales) OVER (ORDER BY month) AS running_total
FROM monthly_sales;
```
Now each row sees all rows from the beginning up to and including the current row.

For example:

- January → January sales
- February → January + February
- March → January + February + March

This creates a cumulative running total.

Without `ORDER BY`, `OVER()` considers the whole table, so every row receives the same grand total.

With `ORDER BY`, the window becomes cumulative, effectively using a frame from the beginning up to the current row.

For a 3-month moving average, we use a fixed window frame:
```
AVG(sales) OVER (
    ORDER BY month
    ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
)
```
This includes the current month and the previous two months, creating a 3-month window that moves forward through the data.

**The Bonus:**

`ORDER BY` inside `OVER()` changes the window from a flat whole-table window into a cumulative window by defining the order and how far back the calculation looks.

### 📝 Query:

Running Total
```
SELECT month, sales,
    SUM(sales) OVER (ORDER BY month) AS running_total
FROM monthly_sales;
```
**3-Month Moving Average**
```
SELECT month, sales,
    AVG(sales) OVER (
        ORDER BY month
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS moving_avg
FROM monthly_sales;
```

## ⭐ Key Takeaways:

- `ORDER BY` inside `OVER()` can turn a flat total into a running total.
- `SUM(sales) OVER ()` gives the same grand total on every row.
- `SUM(sales) OVER (ORDER BY month)` creates a cumulative running total.
- A window frame controls how many rows are included in a calculation.
- `ROWS BETWEEN 2 PRECEDING AND CURRENT ROW` creates a 3-row moving window.
- Running totals grow over time, while moving averages slide across a fixed window.
