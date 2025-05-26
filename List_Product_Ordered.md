# LeetCode SQL: List the Products Ordered in a Period

**Link to the problem:**  
[https://leetcode.com/problems/list-the-products-ordered-in-a-period](https://leetcode.com/problems/list-the-products-ordered-in-a-period)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given two tables:

- `Products`: Contains `product_id` and `product_name`
- `Orders`: Contains `product_id`, `unit`, and `order_date`

Your task is to return the names of products ordered **at least 100 units** during the month of **February 2020**. The result should include the product name and the total units ordered.

## SQL Solution

```sql
SELECT 
    p.product_name, 
    SUM(unit) AS unit
FROM 
    products p 
INNER JOIN 
    orders o 
ON 
    p.product_id = o.product_id
WHERE 
    order_date BETWEEN '2020-02-01' AND '2020-02-29'
GROUP BY 
    p.product_name
HAVING 
    unit >= 100;
