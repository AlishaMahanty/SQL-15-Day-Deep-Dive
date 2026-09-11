# Day 13 - Ranking Functions

## 📌 Problem:

You rank salespeople by sales within each region. Two of them tie for 2nd place.

Depending on which ranking function you choose, the positions come out differently:
```
1, 2, 2, 4

1, 2, 2, 3

1, 2, 3, 4
```
Three functions handle these ties differently: `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()`. Choosing the wrong one can change the result.

### ❓ The question is:

Which function produces which sequence, and which one should you use to pick exactly one top performer per region?

**Bonus Question:**

To get the top 2 products in every category, why can't you just use `LIMIT 2`?

## 💡 Solution:

All three ranking functions use:
```
OVER (PARTITION BY region ORDER BY sales DESC)
```
The difference is how they handle ties.

`ROW_NUMBER()`
```
1, 2, 3, 4`
```
Every row receives a unique number. Ties are broken arbitrarily.

Use ROW_NUMBER() when you need to select exactly one row per group or deduplicate records.

`RANK()`
```
1, 2, 2, 4
```
Tied rows receive the same rank, and the next rank is skipped.

`DENSE_RANK()`
```
1, 2, 2, 3
```
Tied rows receive the same rank, but there is no gap after the tie.

Therefore, to pick one top performer per region, use `ROW_NUMBER()` and filter for rank 1.

For the bonus, `LIMIT 2` cannot return the top 2 products for every category because `LIMIT` is applied globally to the result. A window function allows the ranking to restart within each category.

**The Bonus:**

`LIMIT 2` returns only 2 rows from the entire result, not 2 rows from each category.

For a `Top-N` per Group problem, use a window function such as `ROW_NUMBER()`.

### 📝 Query:

**Pick exactly one top performer per region**
```
WITH ranked AS (
    SELECT
        salesperson,
        region,
        sales,
        ROW_NUMBER() OVER (
            PARTITION BY region
            ORDER BY sales DESC
        ) AS rn
    FROM sales
)
SELECT *
FROM ranked
WHERE rn = 1;
```
**Top 2 products in every category**
```
WITH ranked AS (
    SELECT
        product,
        category,
        sales,
        ROW_NUMBER() OVER (
            PARTITION BY category
            ORDER BY sales DESC
        ) AS rn
    FROM products
)
SELECT *
FROM ranked
WHERE rn <= 2;
```

## ⭐ Key Takeaways:

- `ROW_NUMBER()` gives every row a unique rank.
- `RANK()` gives tied rows the same rank and leaves gaps.
- `DENSE_RANK()` gives tied rows the same rank without gaps.
- Use `ROW_NUMBER()` when you need exactly one top performer per group.
- `LIMIT` works globally, not separately within each group.
- `Top-N` per Group is a common window-function pattern.
