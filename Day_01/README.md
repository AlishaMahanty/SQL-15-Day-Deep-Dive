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

---

## 💡 Solution:

SQL does not execute queries from top to bottom. It follows a logical execution order:

FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY

When the `WHERE` clause runs, the `SELECT` clause has not executed yet. Therefore, the alias `revenue` does not exist at that stage.

That is why this fails:

```
WHERE revenue > 100000
```

The alias is created later during the `SELECT` phase.

However, `ORDER BY` runs after `SELECT`, so the alias is already available:

```
ORDER BY revenue DESC
```

To filter using the calculated value, repeat the expression:

```
WHERE price * quantity > 100000
```

or calculate it earlier using a CTE.

**The bonus:** 

WHERE runs first, SELECT runs later. Always.

### 📝 Query:

Incorrect query
```
SELECT 
    product,
    price * quantity AS revenue
FROM sales
WHERE revenue > 100000;
```

Correct query
```
SELECT 
    product,
    price * quantity AS revenue
FROM sales
WHERE price * quantity > 100000
ORDER BY revenue DESC;
```

## ⭐ Key Takeaways:
- SQL does not execute queries line-by-line.
- WHERE runs before SELECT, so it cannot access aliases created in SELECT.
- ORDER BY runs after SELECT, so it can use column aliases.
- Understanding SQL execution order helps debug common query errors.
