# Day 09 - Common Table Expressions (CTEs)

## 📌 Problem:

You need to find products that made 500%+ profit and have a rating below the average.

The query works, but the calculation is buried inside a nested subquery:
```
SELECT *
FROM (
    SELECT 
        product,
        (revenue - cost) / cost * 100 AS profit_pct,
        rating
    FROM products
) x
WHERE x.profit_pct > 500
  AND x.rating < (SELECT AVG(rating) FROM products);
```
As more conditions are added, the query becomes increasingly difficult to read and maintain.

### ❓ The question is:

How do you rewrite this query so that it reads from top to bottom like a series of clear steps?

**Bonus Question:**

After the query ends with a semicolon, can you reuse the named `CTE` in your next query?

## 💡 Solution:

A `CTE` (Common Table Expression) uses the `WITH` clause to name a temporary result and then query it like a normal table.

Instead of nesting the calculation inside another `SELECT`, define it first:
```
WITH profit AS (
    SELECT 
        product,
        (revenue - cost) / cost * 100 AS profit_pct,
        rating
    FROM products
)
SELECT product
FROM profit
WHERE profit_pct > 500
  AND rating < (SELECT AVG(rating) FROM products);
```
Now the query reads from top to bottom:

- Calculate the profit percentage and create the profit result.
- Filter products with profit above 500%.
- Keep products rated below the average.

`CTEs` can also be chained together using commas:
```
WITH step1 AS (...),
     step2 AS (...)
SELECT ...
```
**The Bonus:**
The `CTE` exists only for the statement in which it is defined. Once the query ends with a semicolon, the `CTE` cannot be reused in the next query. If you need a reusable result, you would use a `VIEW`.

### 📝 Query:

**Query with the Issue**
```
SELECT *
FROM (
    SELECT 
        product,
        (revenue - cost) / cost * 100 AS profit_pct,
        rating
    FROM products
) x
WHERE x.profit_pct > 500
  AND x.rating < (SELECT AVG(rating) FROM products);
```
**Correct Query**
```
WITH profit AS (
    SELECT 
        product,
        (revenue - cost) / cost * 100 AS profit_pct,
        rating
    FROM products
)
SELECT product
FROM profit
WHERE profit_pct > 500
  AND rating < (SELECT AVG(rating) FROM products);
```

## ⭐ Key Takeaways:

- A `CTE` uses the `WITH` clause to create a named temporary result.
- `CTEs` can make complex queries easier to read and maintain.
- `CTEs` allow complex logic to be broken into clear, sequential steps.
- Multiple `CTEs` can be chained together using commas.
- A `CTE` exists only within the statement where it is defined.
- A `CTE` improves readability but does not automatically make a query faster.
