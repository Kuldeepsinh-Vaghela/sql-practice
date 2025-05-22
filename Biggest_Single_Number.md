# LeetCode SQL: Biggest Single Number

**Link to the problem:**  
[https://leetcode.com/problems/biggest-single-number](https://leetcode.com/problems/biggest-single-number)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given a `MyNumbers` table with a single column `num`. The task is to find the **largest number that appears only once** in the table. If no such number exists, return `null`.

## SQL Solution

```sql
WITH num_cnt AS (
    SELECT 
        num, 
        COUNT(num) AS cnt_num
    FROM 
        mynumbers
    GROUP BY 
        num
    HAVING 
        cnt_num = 1
)

SELECT 
    MAX(num) AS num
FROM 
    num_cnt;
