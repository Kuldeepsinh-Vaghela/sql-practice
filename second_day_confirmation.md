# TikTok User Sign-Up Confirmation Analysis

## Problem Statement:
New users on TikTok sign up using their **email addresses**. After signing up, each user receives a **text message confirmation** to activate their account. Your task is to **find users who did not confirm their sign-up on the first day but confirmed on the second day**.

---

## Definition:

- **`action_date`** refers to the date when users activated their accounts and confirmed their sign-up via text message.
- We need to identify users who:
  1. Signed up on **Day 1** but did **not confirm** on the same day.
  2. Confirmed their sign-up on **Day 2**.

---

## Table: `emails`

| Column Name | Type      | Description                          |
|------------|----------|--------------------------------------|
| email_id   | integer  | Unique identifier for the email.    |
| user_id    | integer  | Unique identifier for the user.     |
| signup_date | datetime | Date and time of user sign-up.     |

---

## Table: `texts`

| Column Name    | Type      | Description                                          |
|---------------|----------|------------------------------------------------------|
| text_id       | integer  | Unique identifier for the text message.             |
| email_id      | integer  | Foreign key referencing `emails.email_id`.           |
| signup_action | string   | Status of sign-up confirmation (`Confirmed`, `Not Confirmed`). |
| action_date   | datetime | Date and time of the confirmation action.           |

---

### Example Input:

#### `emails` Table:

| email_id | user_id | signup_date           |
|----------|--------|----------------------|
| 125      | 7771   | 06/14/2022 00:00:00  |
| 433      | 1052   | 07/09/2022 00:00:00  |

#### `texts` Table:

| text_id | email_id | signup_action  | action_date          |
|---------|---------|---------------|----------------------|
| 6878    | 125     | Confirmed      | 06/14/2022 00:00:00 |
| 6997    | 433     | Not Confirmed  | 07/09/2022 00:00:00 |
| 7000    | 433     | Confirmed      | 07/10/2022 00:00:00 |

---

### Example Output:

| user_id |
|---------|
| 1052    |

---

### Explanation:

1. **User 7771** confirmed their sign-up on the **same day (06/14/2022)**, so they are **not included** in the output.
2. **User 1052** signed up on **07/09/2022** but **did not confirm** on the same day.
3. **User 1052** confirmed their sign-up on **07/10/2022** (the second day), so they are **included** in the output.

Thus, the final result shows **only user 1052** as they confirmed their sign-up **on the second day**.

## SQL Query
``` sql
WITH delayed_emailid as (
  SELECT 
    email_id,
    sum(CASE WHEN signup_action = 'Confirmed' or signup_action = 'Not confirmed' THEN 1 ELSE 0 END) delayed_signup
  FROM texts
  GROUP BY email_id
  
)

SELECT e.user_id
FROM emails e LEFT JOIN delayed_emailid de
on e.email_id = de.email_id
where de.delayed_signup = 2;