# Day 11 - Conditional Aggregation

## 📌 Problem:

Your sales table has one row per sale, with a month column containing `Jan`, `Feb`, and `Mar`.

Your manager wants a report with one row per product and three separate columns: Jan sales, Feb sales, and Mar sales side by side. This is a pivot.

### ❓ The uestion is:

How do you turn rows into columns in pure SQL, using `CASE`?

**Bonus Question:**

What does a `CASE` return when there is no matching `WHEN` and no `ELSE`?

## 💡 Solution:

`CASE` works like an if/else statement in SQL.

It can be used to create labels at the row level:
```
CASE
    WHEN score >= 90 THEN 'A'
    WHEN score >= 75 THEN 'B'
    ELSE 'C'
END
```
But the more powerful technique is conditional aggregation using `SUM(CASE...)`. This allows us to turn values stored in rows into separate columns.
```
SELECT product,
    SUM(CASE WHEN month = 'Jan' THEN sales ELSE 0 END) AS jan,
    SUM(CASE WHEN month = 'Feb' THEN sales ELSE 0 END) AS feb,
    SUM(CASE WHEN month = 'Mar' THEN sales ELSE 0 END) AS mar
FROM sales
GROUP BY product;
```
`Each SUM(CASE...)` only adds sales for its respective month, so the month values become separate columns.

One important trap is that `CASE` stops at the first `WHEN` condition that is `TRUE`, so the order of conditions matters.

For example, `WHEN score >= 90` should come before `WHEN score >= 75`. Otherwise, a score of 95 would be caught by the 75 condition first.

**The Bonus:**

If no `WHEN` condition matches and there is no `ELSE`, `CASE` returns `NULL`.

### 📝 Query:

**Solution Query**
```
SELECT product,
    SUM(CASE WHEN month = 'Jan' THEN sales ELSE 0 END) AS jan,
    SUM(CASE WHEN month = 'Feb' THEN sales ELSE 0 END) AS feb,
    SUM(CASE WHEN month = 'Mar' THEN sales ELSE 0 END) AS mar
FROM sales
GROUP BY product;
```
CASE Example
```
CASE
    WHEN score >= 90 THEN 'A'
    WHEN score >= 75 THEN 'B'
    ELSE 'C'
END
```

## ⭐ Key Takeaways:
- `CASE` works like an if/else statement in SQL.
- `SUM(CASE...)` is a powerful technique for conditional aggregation.
- Conditional aggregation can be used to turn rows into columns.
- Each `SUM(CASE...)` aggregates only the rows matching its condition.
- The order of `WHEN` conditions matters because `CASE` stops at the first `TRUE` condition.
- A `CASE` with no matching `WHEN` and no `ELSE` returns `NULL`.
