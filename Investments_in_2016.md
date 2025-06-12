# LeetCode SQL: Investments in 2016

**Problem Link:**  
[https://leetcode.com/problems/investments-in-2016](https://leetcode.com/problems/investments-in-2016/?envType=study-plan-v2&envId=top-sql-50)

---

## 🧩 Problem Description

You are given a table `Insurance` with investment records.

### `Insurance` Table  
| Column     | Type |
|------------|------|
| pid        | int  |
| tiv_2015   | float|
| tiv_2016   | float|
| lat        | float|
| lon        | float|

Find the total `tiv_2016` value for policies that:
1. Have a `tiv_2015` value **shared** with at least one other policy.
2. Do **not** share both `lat` and `lon` with any other policy.

Return the sum rounded to two decimal places.

---

## ✅ Solution Using CTEs

```sql
WITH sme_tiv2015_val AS (
    SELECT DISTINCT l.pid, l.tiv_2015, l.tiv_2016, l.lat, l.lon
    FROM Insurance l, Insurance r
    WHERE l.pid != r.pid AND l.tiv_2015 = r.tiv_2015
    ORDER BY l.pid
),

rem_cond_2 AS (
    SELECT DISTINCT l.pid, l.tiv_2015, l.tiv_2016, l.lat, l.lon
    FROM sme_tiv2015_val l, Insurance r
    WHERE l.pid != r.pid AND l.lat = r.lat AND l.lon = r.lon
)

SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM sme_tiv2015_val
WHERE pid NOT IN (SELECT pid FROM rem_cond_2);
