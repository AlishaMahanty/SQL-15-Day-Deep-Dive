# Day 10 - Correlated Subqueries

## 📌 Problem:

Find every employee who earns more than the average salary in their own department.

Sales has its own average salary, Engineering has another, and Marketing has another. Each employee must be compared against the average salary of their own department.

A plain subquery like this:
```
WHERE salary > (SELECT AVG(salary) FROM employees)
```
Uses one company-wide average, which is the wrong benchmark.

### ❓ The question is:

How do you compare each employee's salary to an average that changes based on their department, inside one query?

**Bonus Question:**

This style of subquery re-runs once for every row. On a million-row table, what is the performance risk?

## 💡 Solution:

A correlated subquery is a subquery that refers back to a column from the outer query.
```
SELECT name, department, salary
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE department = e.department
);
```
The key part is:
```
department = e.department
```
Here, `e.department` refers to the department of the current employee from the outer query.

Therefore, the inner query calculates the average salary for that employee's department, rather than calculating one company-wide average.

That is what makes the subquery correlated.

A regular subquery can run once and return a value. A correlated subquery can run once for each outer row, similar to a `for` loop processing employees one by one.

**The Bonus:**

On a million rows it can fire a million inner queries. That is the risk.

### 📝 Query:

**Query with the Issue**
```
SELECT name, department, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```
This compares every employee against the company-wide average, not their department average.

**Correct query**
```
SELECT name, department, salary
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE department = e.department
);
```

## ⭐ Key Takeaways:

- A correlated subquery refers to a column from the outer query.
- It allows each row to be compared against a value specific to that row.
- A regular subquery can calculate one common value.
- A correlated subquery can execute once for each outer row.
- Correlated subqueries can create performance risks on very large tables.
- A window function such as `AVG(salary) OVER (PARTITION BY department)` can often perform the same analysis more efficiently.
- Use `EXPLAIN ANALYZE` to compare query performance rather than assuming which approach is faster.
