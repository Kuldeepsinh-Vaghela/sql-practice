# LeetCode SQL: Immediate Food Delivery II

**Link to the problem:**  
[https://leetcode.com/problems/immediate-food-delivery-ii](https://leetcode.com/problems/immediate-food-delivery-ii)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You are given a `Delivery` table that tracks `customer_id`, `order_date`, and `customer_pref_delivery_date`. Your task is to calculate the **percentage of customers who received an immediate delivery for their first order**, rounded to 2 decimal places.

## SQL Solution

```sql
WITH cust_imm_sch_or AS (
    SELECT 
        customer_id, 
        order_date,
        CASE
            WHEN order_date = customer_pref_delivery_date  
            THEN 'immediate' 
            ELSE 'scheduled'
        END AS order_info,
        RANK() OVER(PARTITION BY customer_id ORDER BY order_date) AS rank_no        
    FROM delivery
    ORDER BY customer_id, order_date
),

cust_first_order AS (
    SELECT * 
    FROM cust_imm_sch_or
    WHERE rank_no = 1
)

SELECT 
    ROUND(
        SUM(CASE WHEN order_info = 'immediate' THEN 1 ELSE 0 END) 
        / COUNT(*) * 100, 2
    ) AS immediate_percentage
FROM cust_first_order;
