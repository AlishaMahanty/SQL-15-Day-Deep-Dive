# Day 06 - LEFT JOIN and WHERE

## 📌 Problem:

You want a list of All customers with their orders, including customers who have never placed an order.

So you use a `LEFT JOIN`:
```
SELECT c.customer, o.order_id
FROM customers c
LEFT JOIN orders o 
    ON o.customer_id = c.customer_id
WHERE o.status = 'delivered';
```

However, customers who have never ordered suddenly disappear from the result.

You never removed the `LEFT JOIN`, but it no longer behaves like one.

### ❓ The question is:

Why does the `WHERE` condition turn the `LEFT JOIN` into an effective `INNER JOIN`?

**Bonus Question:**

If there are 100 customers and 30 have never ordered, what happens when you add:

```sql
WHERE o.order_date > '2024-01-01'
```

How many customers roughly survive?

## 💡 Solution:

A `LEFT JOIN` keeps every row from the left table.

For customers who have no orders, the columns from the `orders` table contain `NULL`.

After the join, this condition runs:

```sql
WHERE o.status = 'delivered'
```

For customers with no orders:

```text
NULL = 'delivered' → UNKNOWN
```

`WHERE` only keeps rows where the condition is `TRUE`. Therefore, customers with no orders are removed.

The `LEFT JOIN` effectively behaves like an `INNER JOIN`.

If you want to keep the `LEFT JOIN` and only include delivered orders, move the condition into the `ON` clause:

```sql
LEFT JOIN orders o 
    ON o.customer_id = c.customer_id
    AND o.status = 'delivered'
```

Another option is to explicitly allow the `NULL` values:

```sql
WHERE o.status = 'delivered'
   OR o.status IS NULL
```

**The bonus:**

If 30 customers never ordered, a `WHERE` condition on a column from the right table removes those 30 customers. Roughly **70 customers** survive.

### 📝 Query:

**Query with the Issue**

```sql
SELECT c.customer, o.order_id
FROM customers c
LEFT JOIN orders o 
    ON o.customer_id = c.customer_id
WHERE o.status = 'delivered';
```

**Correct Query**

```sql
SELECT c.customer, o.order_id
FROM customers c
LEFT JOIN orders o 
    ON o.customer_id = c.customer_id
    AND o.status = 'delivered';
```

**Alternative Correct Query**

```sql
SELECT c.customer, o.order_id
FROM customers c
LEFT JOIN orders o 
    ON o.customer_id = c.customer_id
WHERE o.status = 'delivered'
   OR o.status IS NULL;
```

## ⭐ Key Takeaways:

* A `LEFT JOIN` keeps all rows from the left table.
* Unmatched rows from the right table contain `NULL`.
* A `WHERE` condition on a right-table column can remove those `NULL` rows.
* This can make a `LEFT JOIN` behave like an `INNER JOIN`.
* When filtering the right table while preserving unmatched rows, put the condition in the `ON` clause.
* Always consider where a filter is applied when working with outer joins.

