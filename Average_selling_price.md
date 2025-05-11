# Average Selling Price

**LeetCode Problem:** [Average Selling Price](https://leetcode.com/problems/average-selling-price/?envType=study-plan-v2&envId=top-sql-50)

## 📘 Problem Description

Given two tables: `Prices` and `UnitsSold`, calculate the **average selling price** per `product_id`.

- The `Prices` table defines the price of a product during a date range (`start_date` to `end_date`).
- The `UnitsSold` table contains individual records for product sales on specific `purchase_date`s.

Return a result that shows the `product_id` and its **average selling price**, calculated as:


If a product has **no units sold**, its average price should be **0**.

---

## 🧾 Table Schema

### Prices

| Column Name | Type  |
|-------------|--------|
| product_id  | int    |
| start_date  | date   |
| end_date    | date   |
| price       | int    |

- Composite Primary Key: `(product_id, start_date, end_date)`
- No overlapping price periods for a product.

### UnitsSold

| Column Name   | Type  |
|----------------|--------|
| product_id     | int    |
| purchase_date  | date   |
| units          | int    |

---

## 📊 Sample Data

### Prices

| product_id | start_date | end_date   | price |
|------------|------------|------------|-------|
| 1          | 2020-01-01 | 2020-01-31 | 5     |
| 1          | 2020-02-01 | 2020-02-29 | 20    |
| 2          | 2020-01-15 | 2020-02-15 | 15    |

### UnitsSold

| product_id | purchase_date | units |
|------------|----------------|--------|
| 1          | 2020-01-05     | 100    |
| 1          | 2020-02-10     | 50     |
| 2          | 2020-02-10     | 200    |
| 2          | 2020-02-10     | 100    |

---

## ✅ Expected Output

| product_id | average_price |
|------------|----------------|
| 1          | 10.00          |
| 2          | 15.00          |

---

## ✅ Final SQL Solution

```sql
WITH cleaned_prod_unt_pr AS (
    SELECT 
        COALESCE(us.product_id, p.product_id) AS product_id, 
        CASE 
            WHEN us.units > 0 THEN us.units
            ELSE NULL
        END AS cleaned_units,
        CASE 
            WHEN p.price > 0 THEN p.price
            ELSE NULL
        END AS cleaned_price
    FROM UnitsSold us
    JOIN Prices p
        ON us.product_id = p.product_id 
       AND us.purchase_date BETWEEN p.start_date AND p.end_date
)

SELECT 
    product_id, 
    COALESCE(
        ROUND(SUM(cleaned_units * cleaned_price) * 1.0 / SUM(cleaned_units), 2),
        0
    ) AS average_price
FROM cleaned_prod_unt_pr
GROUP BY product_id
ORDER BY product_id;
