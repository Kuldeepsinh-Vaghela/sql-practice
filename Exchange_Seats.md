# LeetCode SQL: Exchange Seats

**Link to the problem:**  
[https://leetcode.com/problems/exchange-seats](https://leetcode.com/problems/exchange-seats)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given a `Seat` table with two columns:
- `id` — the seat number (1-indexed, unique)
- `student` — the name of the student in that seat

Your task is to swap seats for every two consecutive students. If there's an odd number of students, the student in the last seat remains in place.

---

## ✅ Solution: Using `LEAD()` and `CASE`

```sql
WITH lead_id AS (
    SELECT 
        id, 
        student, 
        LEAD(id) OVER (ORDER BY id) AS next_id
    FROM 
        seat
)

SELECT 
    CASE
        WHEN id % 2 = 0 THEN id - 1
        WHEN id % 2 != 0 AND next_id IS NOT NULL THEN next_id
        ELSE id
    END AS id,
    student
FROM 
    lead_id
ORDER BY 
    id;
