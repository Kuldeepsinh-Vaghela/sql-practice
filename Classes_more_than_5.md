# LeetCode SQL: Classes More Than 5 Students

**Link to the problem:**  
[https://leetcode.com/problems/classes-more-than-5-students](https://leetcode.com/problems/classes-more-than-5-students)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You are given a `Courses` table with columns `student` and `class`. The task is to write a query to report all classes that have **five or more students**.

## SQL Solution

```sql
SELECT 
    class
FROM 
    courses
GROUP BY 
    class
HAVING 
    COUNT(student) >= 5;
