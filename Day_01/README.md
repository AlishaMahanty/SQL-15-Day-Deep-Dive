# Day 01 - SQL Execution Order

## 📌 Problem:

You write a SQL query to calculate revenue using an alias:

```
SELECT 
    product,
    price * quantity AS revenue
FROM sales
WHERE revenue > 100000;
```

However, MySQL rejects the query with:

Unknown column `revenue` in `where` clause

Interestingly, using the same alias in `ORDER BY` works perfectly:

```
ORDER BY revenue DESC;
```

### ❓ The question is:

Why does SQL refuse to recognize the `revenue` alias in `WHERE` but accept it in `ORDER BY`?

**Bonus Question:**

In one line, what executes first — `WHERE` or `SELECT`?
