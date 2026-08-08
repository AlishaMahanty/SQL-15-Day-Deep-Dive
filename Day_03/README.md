# Day 03 - LIMIT and Ties

## 📌 Problem:

Your manager asks for the top 5 dealers by sales:
```
SELECT dealer, sales
FROM performance
ORDER BY sales DESC
LIMIT 5;
```
The query works, but later dealer #6 complains that he had the exact same sales as dealer #5, ₹18 lakh each, but he was left out of the list.

The problem is that `LIMIT 5` returns exactly 5 rows, even when the 5th and 6th rows have the same sales.

### ❓ The question is:

Why did `LIMIT` drop a tied row, and how do you return all the dealers who tie for 5th?

**Bonus Question:**

When two rows have identical sales, what decides their order in `ORDER BY sales DESC`?

---

## 💡 Solution:

`LIMIT` counts rows, it does not understand ties.

When you use:
```
LIMIT 5
```
SQL simply takes the first 5 rows and stops. It has no idea that the 5th and 6th dealers have the same sales and are tied.

When two rows have identical values and there is no additional tiebreaker, their order is undefined. The database can return tied rows in any order, and the result can even change between runs.

One way to make the ordering deterministic is to add a tiebreaker:
```
ORDER BY sales DESC, dealer ASC
```
Now dealers with the same sales are ordered alphabetically.

However, if the requirement is to include all dealers tied for 5th place, use a ranking function such as `RANK()` and filter for `rank <= 5`.

**The bonus:**

Without a tiebreaker, the order of tied rows is undefined. Never trust it.

### 📝 Query:

**Query with the Issue**
```
SELECT dealer, sales
FROM performance
ORDER BY sales DESC
LIMIT 5;
```
**Correct Query**

Query with a Tiebreaker
```
SELECT dealer, sales
FROM performance
ORDER BY sales DESC, dealer ASC
LIMIT 5;
```
Query to Keep All Ties for Top 5
```
WITH ranked AS (
    SELECT 
        dealer,
        sales,
        RANK() OVER (ORDER BY sales DESC) AS sales_rank
    FROM performance
)
SELECT dealer, sales
FROM ranked
WHERE sales_rank <= 5;
```

## ⭐ Key Takeaways:

- `LIMIT 5` returns exactly 5 rows; it does not account for ties.
- Tied rows can appear in an arbitrary order when no tiebreaker is specified.
- Add a secondary column to make the ordering deterministic.
- Use `RANK()` when the requirement is to include all rows tied within the `top N` ranks.
- `LIMIT` is for limiting rows; ranking functions are better for `top-N` analysis when ties matter.
