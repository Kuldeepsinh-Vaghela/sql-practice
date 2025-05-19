# LeetCode SQL: Number of Unique Subjects Taught by Each Teacher

**Link to the problem:**  
[https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given a table `Teacher` that contains the `teacher_id` and `subject_id` columns. Each row indicates a subject taught by a specific teacher. The goal is to find the number of **unique subjects** each teacher teaches.

## SQL Solution

```sql
SELECT 
    teacher_id, 
    COUNT(DISTINCT subject_id) AS cnt
FROM 
    teacher
GROUP BY 
    teacher_id
ORDER BY 
    teacher_id;
