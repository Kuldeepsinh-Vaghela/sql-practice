# LeetCode SQL: User Activity for the Past 30 Days I

**Link to the problem:**  
[https://leetcode.com/problems/user-activity-for-the-past-30-days-i](https://leetcode.com/problems/user-activity-for-the-past-30-days-i)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You are given an `Activity` table with `user_id`, `activity_date`, and `activity_type`. Your task is to report the number of **active users** for each day in the **30-day window ending on 2019-07-27**. A user is considered active on a day if they have at least one type of activity on that day.

## SQL Solution

```sql
WITH user_act_grp AS (
    SELECT 
        user_id, 
        activity_date, 
        COUNT(DISTINCT activity_type) AS activity_num
    FROM 
        activity
    GROUP BY 
        user_id, activity_date
    ORDER BY 
        user_id, activity_date
)

SELECT 
    activity_date AS day, 
    COUNT(user_id) AS active_users
FROM 
    user_act_grp
WHERE 
    activity_num > 0 
    AND activity_date > DATE_ADD('2019-07-27', INTERVAL -30 DAY) 
    AND activity_date <= '2019-07-27'
GROUP BY 
    activity_date
ORDER BY 
    activity_date;
