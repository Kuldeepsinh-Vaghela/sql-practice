# LeetCode SQL: Find Followers Count

**Link to the problem:**  
[https://leetcode.com/problems/find-followers-count](https://leetcode.com/problems/find-followers-count)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given a `Followers` table with two columns: `user_id` and `follower_id`. The task is to return a list of users along with the **number of followers** they have, sorted by `user_id`.

## SQL Solution

```sql
SELECT 
    user_id, 
    COUNT(follower_id) AS followers_count
FROM 
    followers
GROUP BY 
    user_id
ORDER BY 
    user_id;
