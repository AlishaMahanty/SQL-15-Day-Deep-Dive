# SQL 15-Day Deep Dive 🚀

A 15-day SQL learning challenge focused on strengthening SQL fundamentals, advanced querying techniques, and analytical problem-solving through practical business scenarios.

This repository documents my participation in the **Codebasics Discord Channel Daily Learning Challenge (July)**, where I practiced SQL concepts by solving real-world query challenges and understanding the logic behind each solution.

---

## 📌 About the Challenge

Each day of this challenge includes:

- A practical SQL problem statement
- The reasoning behind the problem
- A detailed solution explanation
- SQL query implementation

The goal was not only to write queries but to understand **why SQL behaves the way it does** and how these concepts are applied in real-world analytics scenarios.

---

## 🎯 Objectives

- Strengthen SQL query-writing skills
- Improve analytical thinking and problem-solving ability
- Understand SQL execution behavior
- Practice common Data Analyst interview patterns
- Learn techniques used for business reporting and analysis

---

# 📚 Topics Covered

| Day | Topic | Key Concepts |
|-----|-------|--------------|
| Day 01 | SQL Execution Order | Logical query processing, aliases, WHERE vs ORDER BY |
| Day 02 | NULL Handling | NULL values, three-valued logic, IS NULL |
| Day 03 | LIMIT and Ties | Ranking problems, deterministic ordering |
| Day 04 | GROUP BY Fundamentals | Aggregations, grouping rules, data grain |
| Day 05 | WHERE vs HAVING | Row filtering vs group filtering |
| Day 06 | JOIN and Aggregation | Avoiding duplicate calculations and incorrect metrics |
| Day 07 | Subqueries | IN, EXISTS, NOT IN, NULL handling |
| Day 08 | Common Table Expressions | Writing readable and structured queries |
| Day 09 | Correlated Subqueries | Row-level comparisons |
| Day 10 | Conditional Aggregation | CASE statements, SQL pivoting |
| Day 11 | Window Functions | Aggregations without losing row-level details |
| Day 12 | Ranking Functions | ROW_NUMBER, RANK, DENSE_RANK |
| Day 13 | Running Totals | Window frames, cumulative calculations |
| Day 14 | SQL Interview Problems | Second highest salary, duplicate detection |
| Day 15 | Final SQL Challenge | Applying concepts together |

---

# 🧠 Key Learnings

### Understanding SQL Execution Order

One of the important concepts learned was that SQL does not execute queries from top to bottom.

FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY

This explains why aliases created in `SELECT` cannot be used in `WHERE`, but work in `ORDER BY`.

---

### Handling NULL Values Correctly

Learned how NULL represents unknown values and why comparisons with NULL do not return TRUE or FALSE directly, leading to unexpected filtering behavior.

---

### Preventing Incorrect Business Metrics

A key analytics learning was understanding how joins can multiply rows and create incorrect aggregations if the data grain is not considered before calculating metrics.

---

### Advanced Analytical SQL

Practiced window functions for calculations such as percentages, rankings, and running totals while maintaining row-level details.

---

# 🛠️ Skills Practiced

### SQL Concepts
- SELECT statements
- Filtering and sorting
- Aggregate functions
- GROUP BY and HAVING
- Joins
- Subqueries
- CTEs
- CASE statements
- Window functions
- Ranking functions
- Running totals

### Analytical Skills
- Data validation
- Query debugging
- Business logic translation
- KPI calculation
- Problem-solving

---

# 📈 Outcome

By completing this challenge, I improved my ability to:

✅ Write structured SQL queries  
✅ Debug query logic  
✅ Understand advanced SQL concepts  
✅ Solve analytical problems efficiently  
✅ Apply SQL techniques used in Data Analyst roles  

---

⭐ This repository represents my continuous learning journey in SQL and analytics.
