# LeetCode SQL: Customers Who Bought All Products

**Link to the problem:**  
[https://leetcode.com/problems/customers-who-bought-all-products](https://leetcode.com/problems/customers-who-bought-all-products)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You are given two tables:

- `Customer` with columns: `customer_id` and `product_key`
- `Product` with a column: `product_key`

The goal is to find all customers who have **purchased every product** listed in the `Product` table.

---

## ✅ Solution: Using `GROUP_CONCAT` and CTEs

```sql
WITH customer_grouped AS (
    SELECT 
        customer_id, 
        GROUP_CONCAT(DISTINCT product_key ORDER BY product_key) AS g_product_key
    FROM 
        customer
    GROUP BY 
        customer_id
),

product_grouped AS (
    SELECT 
        GROUP_CONCAT(DISTINCT product_key ORDER BY product_key) AS g_product_key
    FROM 
        product
)

SELECT 
    cg.customer_id
FROM 
    customer_grouped cg, 
    product_grouped pg
WHERE 
    cg.g_product_key = pg.g_product_key;
