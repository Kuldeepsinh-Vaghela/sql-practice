# LeetCode SQL: Product Price at a Given Date

**Link to the problem:**  
[https://leetcode.com/problems/product-price-at-a-given-date](https://leetcode.com/problems/product-price-at-a-given-date)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given a `Products` table with the following columns:
- `product_id`
- `new_price`
- `change_date`

The table records changes in product prices over time. Your task is to return the **price of each product as of `2019-08-16`**.

- If a product had a price update **on or before `2019-08-16`**, return the most recent price.
- If a product had **no price updates before or on that date**, return the default price of **10**.

---

## ✅ Solution: Using CTEs, RANK, and UNION

```sql
WITH product_bef_dte AS (
    SELECT 
        product_id,
        new_price AS price,
        change_date,
        RANK() OVER(PARTITION BY product_id ORDER BY change_date) AS rank_value
    FROM 
        products
    WHERE 
        change_date <= '2019-08-16'
),

product_max_rank AS (
    SELECT 
        *, 
        MAX(rank_value) OVER(PARTITION BY product_id) AS max_rank
    FROM 
        product_bef_dte
),

prod_more_than_dte AS (
    SELECT 
        product_id, 
        10 AS new_price,  
        change_date
    FROM 
        products
    WHERE 
        product_id NOT IN (SELECT product_id FROM product_bef_dte)
)

SELECT 
    product_id, 
    price 
FROM 
    product_max_rank
WHERE 
    rank_value = max_rank

UNION 

SELECT 
    product_id, 
    new_price AS price
FROM 
    prod_more_than_dte;
