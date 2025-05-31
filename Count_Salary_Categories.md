# LeetCode SQL: Count Salary Categories

**Link to the problem:**  
[https://leetcode.com/problems/count-salary-categories](https://leetcode.com/problems/count-salary-categories)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You are given an `Accounts` table with the following columns:

- `account_id`
- `income`

You need to count the number of accounts falling into each of the following salary categories:

- **Low Salary**: `income < 20000`
- **Average Salary**: `20000 <= income <= 50000`
- **High Salary**: `income > 50000`

Even if no accounts fall into a category, that category should still appear in the result with a count of `0`.

---

## ✅ Solution: Using CTEs and `LEFT JOIN`

```sql
WITH salary_categories AS (
    SELECT 'Low Salary' AS category
    UNION ALL
    SELECT 'Average Salary' AS category
    UNION ALL
    SELECT 'High Salary' AS category
),

acc_grouped AS (
    SELECT
        CASE
            WHEN income < 20000 THEN 'Low Salary'
            WHEN income >= 20000 AND income <= 50000 THEN 'Average Salary'
            ELSE 'High Salary'
        END AS category,
        COUNT(account_id) AS accounts_count
    FROM 
        accounts
    GROUP BY 
        category
)

SELECT 
    sc.category, 
    COALESCE(ag.accounts_count, 0) AS accounts_count
FROM 
    salary_categories sc 
LEFT JOIN 
    acc_grouped ag
ON 
    sc.category = ag.category;
