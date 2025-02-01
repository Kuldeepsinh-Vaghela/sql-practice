# Snapchat User Activity Breakdown by Age Group

## Problem Statement:
You are tasked with calculating the breakdown of time spent sending versus opening snaps, as a percentage of total time spent on these activities, grouped by age group.

### Notes:
- To calculate the percentages, use the following formulas:
    1. **Percentage of time spent sending**:  
       \(\text{{Time spent sending}} \div (\text{{Time spent sending}} + \text{{Time spent opening}}) \times 100.0\)
    2. **Percentage of time spent opening**:  
       \(\text{{Time spent opening}} \div (\text{{Time spent sending}} + \text{{Time spent opening}}) \times 100.0\)
  
  - Make sure to multiply by **100.0** (not 100) to avoid integer division issues when calculating percentages.
  
- The solution has been updated and optimized as of April 15th, 2023.

---

## Tables

### Table 1: `activities`

| Column Name    | Type     | Description                                |
|----------------|----------|--------------------------------------------|
| activity_id    | integer  | The unique ID for the activity.            |
| user_id        | integer  | The ID of the user who performed the activity. |
| activity_type  | string   | Type of activity ('send', 'open', 'chat'). |
| time_spent     | float    | The time spent on the activity in hours.   |
| activity_date  | datetime | The date when the activity was performed.  |

#### Example Input for `activities`:
| activity_id | user_id | activity_type | time_spent | activity_date         |
|-------------|---------|---------------|------------|-----------------------|
| 7274        | 123     | open          | 4.50       | 06/22/2022 12:00:00   |
| 2425        | 123     | send          | 3.50       | 06/22/2022 12:00:00   |
| 1413        | 456     | send          | 5.67       | 06/23/2022 12:00:00   |
| 1414        | 789     | chat          | 11.00      | 06/25/2022 12:00:00   |
| 2536        | 456     | open          | 3.00       | 06/25/2022 12:00:00   |

### Table 2: `age_breakdown`

| Column Name  | Type   | Description                          |
|--------------|--------|--------------------------------------|
| user_id      | integer| The unique ID of the user.           |
| age_bucket   | string | The age group of the user ('21-25', '26-30', '31-35', etc.) |

#### Example Input for `age_breakdown`:
| user_id | age_bucket |
|---------|------------|
| 123     | 31-35      |
| 456     | 26-30      |
| 789     | 21-25      |

---

## Example Output:

| age_bucket | send_perc | open_perc |
|------------|-----------|-----------|
| 26-30      | 65.40     | 34.60     |
| 31-35      | 43.75     | 56.25     |

### Explanation:

- For the **age bucket 26-30**:
  - The time spent sending snaps was **5.67** hours and the time spent opening snaps was **3.00** hours.
  - The total time spent on these activities is **5.67 + 3 = 8.67** hours.
  - The percentage of time spent sending snaps:  
    \( \frac{5.67}{5.67 + 3} \times 100.0 = 65.40\% \)
  - The percentage of time spent opening snaps:  
    \( \frac{3}{5.67 + 3} \times 100.0 = 34.60\% \)

---

## Task:
Write a query to calculate the percentage of time spent sending and opening snaps for each **age group**, rounding the percentages to **2 decimal places**, and then displaying the output ordered by **age_bucket**.

--- 

## Constraints:
- Ensure that the output includes:
  - The **age_bucket**.
  - The **percentage of time spent sending snaps** (`send_perc`).
  - The **percentage of time spent opening snaps** (`open_perc`).

---

## SQL Query:
```sql
WITH age_act_time AS(
  SELECT
    ab.age_bucket,
    sum(CASE
      WHEN at.activity_type = 'send' THEN at.time_spent 
      ELSE 0
      END) AS send_time,
    sum(CASE
      WHEN at.activity_type = 'open' THEN at.time_spent 
      ELSE 0
      END) AS open_time,
    sum(time_spent) AS total_time
  FROM age_breakdown  ab inner join  activities at
  ON ab.user_id = at.user_id
  WHERE activity_type IN('open','send')
  GROUP BY age_bucket
)

SELECT 
  age_bucket,
  round(100* send_time/total_time,2) AS send_perc,
  round(100* open_time/total_time,2) AS open_perc
FROM age_act_time;
  

  

  
