# LeetCode SQL: Triangle Judgement

**Link to the problem:**  
[https://leetcode.com/problems/triangle-judgement](https://leetcode.com/problems/triangle-judgement)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given a table `Triangle` with three columns: `x`, `y`, and `z`, representing the lengths of the three sides of potential triangles. Your task is to determine for each row whether the three lengths can form a valid triangle.

A triangle is valid if:
- The sum of any two sides is greater than the third side.

## SQL Solution

```sql
SELECT 
    x, 
    y, 
    z,
    CASE 
        WHEN x + y > z AND x + z > y AND y + z > x THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM 
    triangle;
