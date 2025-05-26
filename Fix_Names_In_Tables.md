# LeetCode SQL: Fix Names in a Table

**Link to the problem:**  
[https://leetcode.com/problems/fix-names-in-a-table](https://leetcode.com/problems/fix-names-in-a-table)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given a `Users` table with two columns: `user_id` and `name`. The `name` values may be improperly cased (e.g., "aLice", "BOB", etc.). Your task is to format each name so that:

- The **first letter is uppercase**
- The **remaining letters are lowercase**

Return the result sorted by `user_id`.

## SQL Solution

```sql
SELECT 
    user_id, 
    CONCAT(
        UPPER(SUBSTRING(name, 1, 1)), 
        LOWER(SUBSTRING(name, 2))
    ) AS name
FROM 
    users
ORDER BY 
    user_id;
