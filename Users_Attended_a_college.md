# LeetCode SQL: Percentage of Users Attended a Contest

**Link to the problem:**  
[https://leetcode.com/problems/percentage-of-users-attended-a-contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You are given the following tables:

- `Users` containing `user_id`
- `Register` containing `contest_id` and `user_id`

The goal is to calculate the percentage of users who registered for each contest. The result should be rounded to 2 decimal places and sorted by descending percentage and then by `contest_id`.

## SQL Solution

```sql
SELECT 
    b.contest_id, 
    ROUND(COUNT(b.user_id) / (a.c_id) * 100, 2) AS percentage
FROM 
    Register b, 
    (SELECT COUNT(user_id) AS c_id FROM Users) AS a
GROUP BY 
    b.contest_id
ORDER BY 
    percentage DESC, 
    b.contest_id;
