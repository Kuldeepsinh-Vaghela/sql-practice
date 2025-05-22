# LeetCode SQL: The Number of Employees Which Report to Each Employee

**Link to the problem:**  
[https://leetcode.com/problems/the-number-of-employees-which-report-to-each-employee](https://leetcode.com/problems/the-number-of-employees-which-report-to-each-employee)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given an `Employees` table with the columns: `employee_id`, `name`, `reports_to`, and `age`. Your task is to report the number of employees reporting to each manager (`reports_to`) along with the average age of those employees, rounded to the nearest integer.

## SQL Solution

```sql
WITH man_emp_cont AS (
    SELECT 
        reports_to, 
        COUNT(reports_to) AS num_emp, 
        AVG(age) AS average_age
    FROM 
        employees
    GROUP BY 
        reports_to
)

SELECT 
    e1.employee_id, 
    e1.name, 
    me.num_emp AS reports_count, 
    ROUND(me.average_age) AS average_age
FROM 
    employees e1, 
    man_emp_cont me
WHERE 
    e1.employee_id = me.reports_to
ORDER BY 
    e1.employee_id;
