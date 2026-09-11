# Day 12 - Window Functions

## 📌 Problem:

You want a table that shows each customer's sales and what percentage of the total market that represents, on the same row:
```
Customer | Sales | % of total
```
The problem is that when you use `GROUP BY customer` to calculate each customer's sales, the table is collapsed and the grand total needed for the percentage denominator is lost.

### ❓ The question is:

How do you put a total next to every row without collapsing the rows into groups?

**Bonus Question:**

`PARTITION BY` vs `GROUP BY` — one keeps all your rows, while the other destroys them. Which is which?

## 💡 Solution:

A window function can perform an aggregation without collapsing the rows.

`GROUP BY` combines rows into groups, while a window function with the `OVER()` clause calculates a value for each row while keeping the original rows.

To calculate each customer's percentage of the total:
```
SELECT customer, sales,
    sales * 100.0 / SUM(sales) OVER () AS pct_of_total
FROM customer_sales;
```
`SUM(sales) OVER ()` calculates the total sales across the entire result and makes that total available on every row. This allows each customer's sales to be divided by the same grand total without using a self-join or subquery.

You can also use `PARTITION BY` when the comparison should happen within a specific group.

For example:
```
SUM(sales) OVER (PARTITION BY region)
```
This calculates the total sales for each region while keeping every individual customer row.

**The Bonus:**

`GROUP BY` collapses rows — one row per group.
`PARTITION BY` keeps rows — the aggregate value is added to each row.

### 📝 Query:

**Solution Query**
```
SELECT customer, sales,
    sales * 100.0 / SUM(sales) OVER () AS pct_of_total
FROM customer_sales;
```
**Using PARTITION BY**
```
SELECT customer, region, sales,
    SUM(sales) OVER (PARTITION BY region) AS region_total
FROM customer_sales;
```

## ⭐ Key Takeaways:

- Window functions perform calculations without collapsing rows.
- `GROUP BY` collapses rows into groups.
- `OVER()` allows an aggregate to be calculated across the result while keeping row-level details.
- `SUM(sales) OVER ()` calculates the grand total and makes it available on every row.
- `PARTITION BY` calculates window values within specific groups while keeping individual rows.
- Use window functions when you need row-level details and aggregate information in the same output.
