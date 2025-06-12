# LeetCode SQL: Restaurant Growth

**Problem Link:**  
[https://leetcode.com/problems/restaurant-growth](https://leetcode.com/problems/restaurant-growth/submissions/1661446827/?envType=study-plan-v2&envId=top-sql-50)

---

## 🧩 Problem Description

Given a table `Customer`, which records the amount spent by a customer each day:

### `Customer`  
| Column Name | Type    |
|-------------|---------|
| customer_id | int     |
| visited_on  | date    |
| amount      | int     |

- `customer_id` and `visited_on` are the composite primary key.
- This table contains information about customer transactions.
- Calculate the 7-day rolling average of total amount spent **per day**.
- Output only rows where a full 7-day average can be computed.

---

## ✅ Custom Solution

```sql
WITH daily_sum_data AS (
    SELECT visited_on, SUM(amount) AS daily_sum
    FROM customer
    GROUP BY visited_on
    ORDER BY visited_on
),

amt_avg_amt AS (
    SELECT
        visited_on,
        SUM(daily_sum) OVER (
            ORDER BY visited_on
            ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
        ) AS amount,
        ROUND(AVG(daily_sum) OVER (
            ORDER BY visited_on
            ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
        ), 2) AS average_amount,
        ROW_NUMBER() OVER(ORDER BY visited_on) AS row_value
    FROM daily_sum_data
)

SELECT visited_on, amount, average_amount 
FROM amt_avg_amt 
WHERE row_value >= 7;
