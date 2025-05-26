# LeetCode SQL: Delete Duplicate Emails

**Link to the problem:**  
[https://leetcode.com/problems/delete-duplicate-emails](https://leetcode.com/problems/delete-duplicate-emails)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You are given a `Person` table containing two columns: `id` and `email`. Some emails may appear multiple times in the table. Your task is to **delete all duplicate entries**, keeping only the record with the **smallest `id`** for each unique email.

## SQL Solution

```sql
DELETE FROM person 
WHERE id NOT IN (
    WITH duplicates AS (
        SELECT 
            id, 
            email, 
            RANK() OVER(PARTITION BY email ORDER BY id) AS rank_id
        FROM 
            person
    )
    SELECT 
        id
    FROM 
        duplicates
    WHERE 
        rank_id = 1
);
