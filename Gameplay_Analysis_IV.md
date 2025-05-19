# LeetCode SQL: Game Play Analysis IV

**Link to the problem:**  
[https://leetcode.com/problems/game-play-analysis-iv](https://leetcode.com/problems/game-play-analysis-iv)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given a table `Activity` with columns `player_id`, `event_date`, and other fields. You need to calculate the **fraction of players who logged in on two consecutive days for their first two days of activity**, rounded to 2 decimal places.

## SQL Solution

```sql
WITH player_ranked AS (
    SELECT *, 
           RANK() OVER(PARTITION BY player_id ORDER BY event_date) AS rank_no
    FROM activity
    ORDER BY player_id, event_date
),

player_date_1 AS (
    SELECT player_id, event_date, rank_no
    FROM player_ranked
    WHERE rank_no = 2
),

player_date_2 AS (
    SELECT player_id, event_date, rank_no
    FROM player_ranked
    WHERE rank_no = 1
)

SELECT 
    ROUND(
        SUM(
            CASE 
                WHEN DATEDIFF(p2.event_date, p1.event_date) = 1 THEN 1 
                ELSE 0 
            END
        ) / COUNT(p1.player_id), 
        2
    ) AS fraction
FROM 
    player_date_2 p1 
LEFT JOIN 
    player_date_1 p2 
ON 
    p1.player_id = p2.player_id;
