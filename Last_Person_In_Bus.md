# LeetCode SQL: Last Person to Fit in the Bus

**Link to the problem:**  
[https://leetcode.com/problems/last-person-to-fit-in-the-bus](https://leetcode.com/problems/last-person-to-fit-in-the-bus)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You are given a `Queue` table with the following columns:

- `person_id`
- `person_name`
- `weight`
- `turn` — the order in which people arrive at the bus

The bus has a weight limit of **1000 units**, and people enter the bus in the order of `turn`. Your task is to return the **name of the last person** who can board the bus without exceeding the total weight limit.

---

## ✅ Solution: Using `SUM()` Window Function and CTEs

```sql
WITH sum_weight AS (
    SELECT 
        *, 
        SUM(weight) OVER (
            ORDER BY turn 
            ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
        ) AS sum_weight
    FROM 
        queue
)

SELECT 
    person_name
FROM 
    sum_weight
WHERE 
    sum_weight <= 1000
ORDER BY 
    sum_weight DESC
LIMIT 1;
