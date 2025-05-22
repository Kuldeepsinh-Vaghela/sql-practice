# LeetCode SQL: Employees Whose Manager Left the Company

**Link to the problem:**  
[https://leetcode.com/problems/employees-whose-manager-left-the-company](https://leetcode.com/problems/employees-whose-manager-left-the-company)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You are given an `Employees` table with columns: `employee_id`, `name`, `salary`, and `manager_id`. Some employees may not have a manager listed in the company.

The task is to find the `employee_id` of those:
- Whose salary is **less than 30,000**
- And whose `manager_id` **does not exist** in the list of current employees

Return the result ordered by `employee_id`.

## SQL Solution

```sql
SELECT 
    employee_id
FROM 
    employees
WHERE 
    salary < 30000 
    AND manager_id NOT IN (
        SELECT employee_id FROM employees
    )
ORDER BY 
    employee_id;
