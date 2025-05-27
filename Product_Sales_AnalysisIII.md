# LeetCode SQL: Product Sales Analysis III

**Link to the problem:**  
[https://leetcode.com/problems/product-sales-analysis-iii](https://leetcode.com/problems/product-sales-analysis-iii)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given a `Sales` table containing sales records with the following columns:

- `product_id`
- `year`
- `quantity`
- `price`

Your task is to write a query to report, for each product, the **first year** it was sold along with the corresponding `quantity` and `price`.

---

## ✅ Solution 1: Using `RANK()`

```sql
WITH sales_ranked AS (
    SELECT 
        *, 
        RANK() OVER(PARTITION BY product_id ORDER BY year) AS rank_no
    FROM 
        sales
)

SELECT 
    product_id, 
    year AS first_year, 
    quantity, 
    price
FROM 
    sales_ranked
WHERE 
    rank_no = 1;
 ```

## ✅ Solution 1: Using `MIN`

```sql
WITH sales_ranked AS (
    SELECT 
        product_id, 
        MIN(year) OVER(PARTITION BY product_id) AS min_year
    FROM 
        sales
)

SELECT DISTINCT 
    s.product_id, 
    s.year AS first_year, 
    s.quantity, 
    s.price
FROM 
    sales_ranked sr 
INNER JOIN 
    sales s 
    ON sr.product_id = s.product_id 
WHERE 
    sr.min_year = s.year;
```