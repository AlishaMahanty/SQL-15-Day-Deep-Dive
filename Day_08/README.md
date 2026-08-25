# Day 08 - Subqueries

## 📌 Problem:

You need to find every customer who has never placed an order.

You write a subquery using `NOT IN`:
```
SELECT customer
FROM customers
WHERE customer_id NOT IN (
    SELECT customer_id 
    FROM orders
);
```
However, the query returns zero rows, even though several customers have never placed an order.

Nothing appears to be wrong with the query or the join key.

### ❓ The question is:

What is causing `NOT IN` to return zero rows, and how can you fix the query?

**Bonus Question:**

What are the three shapes a subquery can take — one in `SELECT`, one in `WHERE`, and one in `FROM`?

## 💡 Solution:

The orders table contains a row where `customer_id` is `NULL`.

That single `NULL` causes a problem with `NOT IN`.

For example:
```
customer_id NOT IN (1, 2, NULL)
```
is effectively evaluated as:
```
customer_id != 1
AND customer_id != 2
AND customer_id != NULL
```
The last comparison returns `UNKNOWN` because comparisons with `NULL` result in `UNKNOWN`.

Since the entire condition cannot become `TRUE`, no rows pass the `WHERE` condition.

There are two ways to fix it.

First, remove the `NULL` values from the subquery:
```
WHERE customer_id NOT IN (
    SELECT customer_id
    FROM orders
    WHERE customer_id IS NOT NULL
)
```
A better approach is to use `NOT EXISTS`, which checks each customer row individually and does not have the same `NULL` problem:
```
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = customers.customer_id
)
```
**The bonus:**

The three subquery shapes are:

Scalar subquery → in `SELECT`, returns one value.
`IN / EXISTS` subquery → in `WHERE`, performs a membership test.
Derived table → in `FROM`, treats the subquery result like a table.

### 📝 Query:

**Query with the Issue**
```
SELECT customer
FROM customers
WHERE customer_id NOT IN (
    SELECT customer_id
    FROM orders
);
```
**Correct Query — Remove NULLs**
```
SELECT customer
FROM customers
WHERE customer_id NOT IN (
    SELECT customer_id
    FROM orders
    WHERE customer_id IS NOT NULL
);
```
**Correct Query — Using NOT EXISTS**
```
SELECT customer
FROM customers
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = customers.customer_id
);
```
## ⭐ Key Takeaways:
- A single `NULL` in a `NOT IN` subquery can cause the entire condition to evaluate to `UNKNOWN`.
- `NOT IN` can produce unexpected results when the subquery contains `NULL`.
- Filter out `NULL` values when using `NOT IN`.
- `NOT EXISTS` is a safer alternative when `NULL` values may be present.
- Subqueries can be used in `SELECT`, `WHERE`, and `FROM` for different purposes.
