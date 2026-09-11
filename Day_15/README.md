# Day 15 - SQL Interview Problems

## 📌 Problem:

Two common SQL interview questions that often look simple but have hidden traps:

1. Find the second-highest salary in the employees table.
2. Find duplicate emails in the users table.

A naive solution such as `ORDER BY salary DESC LIMIT 1,1` fails when the highest salary is shared by two employees because it returns the second row, not the second distinct salary.

### ❓ The question is:

Solve both problems.

For the second-highest salary, what exactly goes wrong with `LIMIT 1,1` or a plain `MAX()`?

**Bonus Question:**

Does your second-highest salary solution still work when two people share the top salary?

## 💡 Solution:

**Second-Highest Salary**

The problem with:
```
ORDER BY salary DESC
LIMIT 1,1;
```
is that `LIMIT 1,1` returns the second row, not the second distinct salary.

For example, if two employees earn ₹1L, the second row can still have a salary of ₹1L.

A robust approach is to find the maximum salary that is less than the highest salary:
```
SELECT MAX(salary)
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
);
```
This returns the true second-highest distinct salary.

Another approach is to use `DENSE_RANK() = 2`, which directly identifies the second distinct salary.

**Duplicate Emails**

Duplicate detection can be solved using `GROUP BY` and `HAVING`:
```
SELECT email, COUNT(*)
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```
The idea is simple: group records by email and keep only the groups containing more than one record.

**The Bonus:**

Yes. Both the max-below-max approach and `DENSE_RANK() = 2` return the true second distinct salary even when two employees share the highest salary.

The `LIMIT` approach does not.

### 📝 Query:

**Query with the Issue — Second-Highest Salary**
```
SELECT salary
FROM employees
ORDER BY salary DESC
LIMIT 1, 1;
```
This returns the second row, not necessarily the second distinct salary.

**Correct Query — Second-Highest Salary**
```
SELECT MAX(salary)
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
);
```
**Correct Query — Duplicate Emails**
```
SELECT email, COUNT(*)
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```
**Alternative — Using DENSE_RANK()**
```
SELECT salary
FROM (
    SELECT
        salary,
        DENSE_RANK() OVER (ORDER BY salary DESC) AS salary_rank
    FROM employees
) ranked
WHERE salary_rank = 2;
```

## ⭐ Key Takeaways:

- `LIMIT 1,1` returns the second row, not the second distinct salary.
- Finding the second-highest salary requires handling ties correctly.
- `MAX(salary)` below the overall maximum gives the second distinct salary.
- `DENSE_RANK()` can also identify the second distinct salary.
- `GROUP BY` and `HAVING` are fundamental tools for finding duplicates.
- Many SQL interview problems are combinations of fundamental SQL concepts.
- Execution order, `NULL` handling, `GROUP BY`, `HAVING`, and window functions all come together in interview-style problems.
