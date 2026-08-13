# Day 05 - WHERE vs HAVING

## 📌 Problem:

You need every city whose total sales exceed ₹10 lakh. You write:
```
SELECT city, SUM(sales)
FROM orders
GROUP BY city
WHERE SUM(sales) > 1000000;
```
The query returns an error: 
``Invalid use of group function``

### ❓ The question is:

Why can't `WHERE` filter on `SUM(sales)`, and which clause is built to do exactly that?

**Bonus Question:**

Do `WHERE region = 'South'` and `HAVING region = 'South'` return the same result? Which one is faster?

---

## 💡 Solution:

`WHERE` filters individual rows, while `HAVING` filters groups after aggregation.

The logical execution order is:

``FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY``

`WHERE` runs before `GROUP BY`. At that point, the groups do not exist yet, so `SUM(sales)` has not been calculated.

That is why this fails:
```
WHERE SUM(sales) > 1000000
```
`HAVING` runs after `GROUP BY`, when the aggregated values are available.

Therefore, use:
```
SELECT city, SUM(sales)
FROM orders
GROUP BY city
HAVING SUM(sales) > 1000000;
```
**The bonus:**

`WHERE region = 'South'` and `HAVING region = 'South'` can return the same rows in this case, but `WHERE` is faster because it filters the rows before grouping. This leaves fewer rows for the aggregation step.

### 📝 Query:

**Incorrect Query**
```
SELECT city, SUM(sales)
FROM orders
GROUP BY city
WHERE SUM(sales) > 1000000;
```
**Correct Query**
```
SELECT city, SUM(sales)
FROM orders
GROUP BY city
HAVING SUM(sales) > 1000000;
```
## ⭐ Key Takeaways:

- `WHERE` filters individual rows before grouping.
- `HAVING` filters groups after aggregation.
- Aggregate functions such as `SUM()` cannot be used in `WHERE` for this purpose.
- Use `WHERE` when the condition applies to individual rows.
- Use `HAVING` when the condition applies to an aggregated result.
- Filtering with `WHERE` earlier can reduce the amount of data that needs to be aggregated.
