# Click-Through Rate (CTR) Calculation for Facebook App Analytics

## Problem Statement:
You have an `events` table that stores analytics data for Facebook apps. Each event can either be an **impression** (when the ad is shown) or a **click** (when the ad is clicked). You need to calculate the **Click-Through Rate (CTR)** for each app in 2022 and round the result to **two decimal places**.

---

## Definition:

The **Click-Through Rate (CTR)** is calculated using the formula:

\[
CTR = \frac{\text{Number of Clicks}}{\text{Number of Impressions}} \times 100.0
\]

### Notes:
- Ensure that the calculation is performed for the year **2022 only**.
- Multiply by `100.0` instead of `100` to avoid integer division.
- The CTR should be **rounded to two decimal places**.

---

## Table: `events`

| Column Name | Type      | Description                                   |
|------------|----------|-----------------------------------------------|
| app_id     | integer  | Unique identifier for the app.                |
| event_type | string   | Type of event (`impression` or `click`).       |
| timestamp  | datetime | Date and time when the event occurred.         |

---

### Example Input:

| app_id | event_type  | timestamp           |
|--------|------------|---------------------|
| 123    | impression | 07/18/2022 11:36:12 |
| 123    | impression | 07/18/2022 11:37:12 |
| 123    | click      | 07/18/2022 11:37:42 |
| 234    | impression | 07/18/2022 14:15:12 |
| 234    | click      | 07/18/2022 14:16:12 |

---

### Example Output:

| app_id | ctr   |
|--------|------|
| 123    | 50.00 |
| 234    | 100.00 |

---

### Explanation:

#### App 123:
- **Impressions:** 2
- **Clicks:** 1
- **CTR Calculation:**  
  \[
  \left(\frac{1}{2}\right) \times 100.0 = 50.00\%
  \]

#### App 234:
- **Impressions:** 1
- **Clicks:** 1
- **CTR Calculation:**  
  \[
  \left(\frac{1}{1}\right) \times 100.0 = 100.00\%
  \]

Thus, the final result displays the **CTR for each app** in **percentage format** rounded to **two decimal places**.


## SQL Query 
```sql
SELECT app_id,
round(100.0 * sum(case when event_type = 'click' then 1 else 0 end) / sum(case when event_type = 'impression' then 1 else 0 end),2) as ctr 
FROM events
WHERE timestamp BETWEEN '2022-01-01' AND '2022-12-31'
group by app_id;