# Day 02 - NULL Handling

## 📌 Problem:

Your `customers` table has 39 rows, including 3 customers whose `city` is blank.

You try to find them using:
```
SELECT * 
FROM customers 
WHERE city = NULL;
```
But the query returns zero rows.

You then try:
```
SELECT * 
FROM customers 
WHERE city != 'Mumbai';
```
The customers with blank cities are missing from the result as well.

### ❓ The question is:

Why does `city = NULL` return nothing, and how do you actually find the blank values?

**Bonus Question:**

Does `WHERE city != 'Mumbai'` include customers whose city is `NULL`, or does it silently drop them?

---

## 💡 Solution:

`NUL`L is not a value. It represents the absence of a known value.

You cannot compare a value to `NULL` using `=` or `!=` and get `TRUE` or `FALSE`.

Comparisons with `NULL` return `UNKNOWN`:
```
city = NULL  → UNKNOWN

city != NULL → UNKNOWN

NULL = NULL  → UNKNOWN
```
`WHERE` only keeps rows that are `TRUE`. Rows that evaluate to `UNKNOWN` are dropped.

That is why this returns zero rows:
```
WHERE city = NULL
```
To actually find the blanks, use the `IS` operator:
```
WHERE city IS NULL
```
To find customers whose city is `not NULL`:
```
WHERE city IS NOT NULL
```
The bonus:

`WHERE city != 'Mumbai'` also silently drops the blank-city customers because:
```
NULL != 'Mumbai' → UNKNOWN
```
If you want to include both non-Mumbai customers and customers whose city is unknown:
```
WHERE city != 'Mumbai' OR city IS NULL
```
### 📝 Query:

Find customers with NULL city
```
SELECT *
FROM customers
WHERE city IS NULL;
Find customers with a known city
SELECT *
FROM customers
WHERE city IS NOT NULL;
```
Exclude Mumbai while keeping NULL values
```
SELECT *
FROM customers
WHERE city != 'Mumbai'
   OR city IS NULL;
```
## ⭐ Key Takeaways:
- `NULL` represents the absence of a value, not an actual value.
- Comparisons such as `= NULL` and `!= NULL` return `UNKNOWN`.
- `WHERE` keeps only rows where the condition is `TRUE`.
- Use `IS NULL` and `IS NOT NULL` to check for `NULL` values.
- `NULL` values can silently disappear from query results when using comparison operators.
- SQL uses three-valued logic: `TRUE`, `FALSE`, and `UNKNOWN`.
