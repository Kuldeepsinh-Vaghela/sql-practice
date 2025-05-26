# LeetCode SQL: Group Sold Products by the Date

**Link to the problem:**  
[https://leetcode.com/problems/group-sold-products-by-the-date](https://leetcode.com/problems/group-sold-products-by-the-date)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given an `Activities` table with at least the following columns: `sell_date` and `product`. Your task is to group the data by `sell_date`, and for each date:

- Return the **number of distinct products sold** (`num_sold`)
- Return the list of **distinct products sold**, in **alphabetical order**, as a comma-separated string (`products`)

## SQL Solution

```sql
SELECT 
    sell_date, 
    COUNT(DISTINCT product) AS num_sold, 
    GROUP_CONCAT(DISTINCT product ORDER BY product) AS products
FROM 
    activities
GROUP BY 
    sell_date;
