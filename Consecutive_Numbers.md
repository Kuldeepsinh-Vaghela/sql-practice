# LeetCode SQL: Consecutive Numbers

**Link to the problem:**  
[https://leetcode.com/problems/consecutive-numbers](https://leetcode.com/problems/consecutive-numbers)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You are given a `Logs` table with a single column `num` and a unique `id` per row (not shown but assumed). Your task is to identify numbers that appear at least **three times in a row**.

## ✅ Solution: Using `ROW_NUMBER` and `LEAD()`

```sql
WITH num_row_number AS (
    SELECT 
        num, 
        ROW_NUMBER() OVER (ORDER BY id ASC) AS row_value
    FROM 
        logs
), 

data_row_num_2 AS (
    SELECT 
        *, 
        LEAD(num) OVER (ORDER BY row_value) AS lead_row_2, 
        LEAD(row_value) OVER (ORDER BY row_value) AS lead_row_value_2, 
        LEAD(num, 2) OVER (ORDER BY row_value) AS lead_row_3, 
        LEAD(row_value, 2) OVER (ORDER BY row_value) AS lead_row_value_3
    FROM 
        num_row_number
)

SELECT DISTINCT 
    num AS ConsecutiveNums
FROM 
    data_row_num_2
WHERE 
    num = lead_row_2 
    AND num = lead_row_3;
