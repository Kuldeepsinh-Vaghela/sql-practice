# 📞 Frequent Callers

**Datalemur Question:** [Frequent Callers](https://datalemur.com/questions/frequent-callers)

---

## 📘 Problem Description

You are given a table `callers` that tracks insurance support calls. Each row represents one call made by a `policy_holder_id`.

Your task is to find out **how many unique policy holders** have made **at least 3 calls**.

Return the final result as a single column: `policy_holder_count`.

---

## 🧾 Table Schema

### callers

| Column Name      | Type    |
|------------------|---------|
| policy_holder_id | int     |
| case_id          | int     |

- Each row is a single call (represented by `case_id`) made by a specific `policy_holder_id`.
- A `policy_holder_id` may appear multiple times, once for each call they’ve made.

---

## 📊 Sample Input

| policy_holder_id | case_id |
|------------------|---------|
| 101              | 1       |
| 101              | 2       |
| 101              | 3       |
| 102              | 4       |
| 103              | 5       |
| 103              | 6       |
| 103              | 7       |
| 104              | 8       |

---

## ✅ Expected Output

| policy_holder_count |
|---------------------|
| 2                   |

In this case, only policy holders `101` and `103` made 3 or more calls.

---

## ✅ SQL Solution

```sql
WITH pol_holder_cc AS (
    SELECT 
        policy_holder_id, 
        COUNT(case_id) AS call_count
    FROM callers
    GROUP BY policy_holder_id
    HAVING COUNT(case_id) >= 3
)

SELECT COUNT(*) AS policy_holder_count
FROM pol_holder_cc;
