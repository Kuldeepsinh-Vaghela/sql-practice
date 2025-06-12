# LeetCode SQL: Friend Requests II – Who Has the Most Friends

**Problem Link:**  
[https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends](https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/solutions/4457460/friend-requests-ii-who-has-the-most-friends/?envType=study-plan-v2&envId=top-sql-50)

---

## 🧩 Problem Description

You're given a table `RequestAccepted` containing friendship requests between users:

### `RequestAccepted`  
| Column        | Type |
|---------------|------|
| requester_id  | int  |
| accepter_id   | int  |
| accept_date   | date |

Find the user who has the **most total friends** (both as requester and accepter combined). Return the `id` and the total number of accepted friend requests they were involved in.

---

## ✅ Solution Using CTEs

```sql
WITH l_col_grpby AS (
    SELECT requester_id AS id, COUNT(requester_id) AS num
    FROM requestaccepted
    GROUP BY requester_id
    ORDER BY id
),

r_col_grpby AS (
    SELECT accepter_id AS id, COUNT(accepter_id) AS num
    FROM requestaccepted
    GROUP BY accepter_id
    ORDER BY id
),

union_l_r AS (
    SELECT * FROM l_col_grpby
    UNION ALL
    SELECT * FROM r_col_grpby
)

SELECT id, SUM(num) AS num
FROM union_l_r
GROUP BY id
ORDER BY num DESC
LIMIT 1;
