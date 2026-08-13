# Day 04 - GROUP BY

## 📌 Problem:

You want total sales per city, but you also include product in the `SELECT`:
```
SELECT city, product, SUM(sales)
FROM orders
GROUP BY city;
```
In MySQL, this may run and show one product per city.

However, in PostgreSQL, SQL Server, or BigQuery, the query returns an error because product is neither grouped nor aggregated.

### ❓ The question is:

Why is putting product in `SELECT` without grouping by it a problem, and which product does MySQL actually show?

**Bonus Question:**

You need totals per city and per city + category. Can one `GROUP BY` give you both?

---

## 💡 Solution:

In a `GROUP BY`, every column is grouped or aggregated.

`GROUP BY` collapses many rows into one row per group.

When you use:
```
GROUP BY city
```
all orders from the same city are combined into a single row.

So, if a city has multiple products, SQL cannot determine which product should appear in that single row.

Every column in the `SELECT` must therefore be either:

- Listed in `GROUP BY`, or
- Wrapped in an aggregate function such as `SUM()`, `COUNT()`, `AVG()`, `MIN()`, or `MAX()`.

In this query:
```
SELECT city, product, SUM(sales)
FROM orders
GROUP BY city;
```
city is grouped and `SUM(sales)` is aggregated, but product is neither.

MySQL may silently return an arbitrary product unless strict mode is enabled, while other databases reject the query.

**The bonus:**

One `GROUP BY` represents one level of data grain. To get totals per city and per city + category, you need separate grouping logic or calculate the finer grain first and roll it up.

📝 Query:

**Query with the Issue**
```
SELECT city, product, SUM(sales)
FROM orders
GROUP BY city;
```
**Correct Query**
```
SELECT city, product, SUM(sales)
FROM orders
GROUP BY city, product;
```
**For Total Sales per City**
```
SELECT city, SUM(sales) AS total_sales
FROM orders
GROUP BY city;
```
**For Total Sales per City and Category**
```
SELECT city, category, SUM(sales) AS total_sales
FROM orders
GROUP BY city, category;
```
## ⭐ Key Takeaways:

- Every non-aggregated column in `SELECT` should be included in `GROUP BY`.
- `GROUP BY` collapses multiple rows into one row per group.
- A column that has multiple values within a group cannot be selected without aggregation.
- MySQL may silently return an arbitrary value when strict grouping is not enabled.
- One `GROUP BY` represents one level of data grain.
- Always understand the required grain before writing an aggregation query.
