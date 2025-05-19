# LeetCode SQL: Project Employees I

**Link to the problem:**  
[https://leetcode.com/problems/project-employees-i](https://leetcode.com/problems/project-employees-i)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given two tables:

- `Project` with columns `project_id` and `employee_id`
- `Employee` with columns `employee_id` and `experience_years`

The goal is to write a query that returns the average years of experience for each project, rounded to 2 decimal places.

## SQL Solution

```sql
SELECT 
    a.project_id, 
    ROUND(AVG(b.experience_years), 2) AS average_years
FROM 
    Project a
LEFT JOIN 
    Employee b 
ON 
    a.employee_id = b.employee_id
GROUP BY 
    a.project_id;
