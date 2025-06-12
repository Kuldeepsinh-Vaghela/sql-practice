# LeetCode SQL: Movie Rating

**Problem Link:**  
[https://leetcode.com/problems/movie-rating](https://leetcode.com/problems/movie-rating/?envType=study-plan-v2&envId=top-sql-50)

---

## 🧩 Problem Description

You are given two tables:

### `Movie`  
| Column Name | Type    |
|-------------|---------|
| movie_id    | int     |
| title       | varchar |

### `Rating`  
| Column Name | Type    |
|-------------|---------|
| user_id     | int     |
| movie_id    | int     |
| rating      | int     |
| created_at  | date    |

The goal is to:

1. Find the name of the user who rated the most number of movies. If there is a tie, return the user with the **lexicographically smallest name**.
2. Find the name of the movie with the highest average rating in **February 2020**. If there is a tie, return the movie with the **lexicographically smallest title**.

---

## ✅ Custom Solution

```sql
WITH user_movie_rating_count AS (
    SELECT mr.user_id, u.name, COUNT(movie_id) AS movie_count
    FROM movierating mr
    LEFT JOIN users u ON mr.user_id = u.user_id
    GROUP BY mr.user_id, u.name
),

max_movie_rtd_usr AS (
    SELECT *
    FROM user_movie_rating_count
    WHERE movie_count = (SELECT MAX(movie_count) FROM user_movie_rating_count)
),

part_1_ans AS (
    SELECT MIN(name) AS results
    FROM max_movie_rtd_usr
),

movie_rating AS (
    SELECT mr.movie_id, m.title, AVG(mr.rating) AS avg_rating
    FROM movierating mr
    LEFT JOIN movies m ON mr.movie_id = m.movie_id
    WHERE mr.created_at BETWEEN '2020-02-01' AND '2020-02-29'
    GROUP BY mr.movie_id, m.title
),

highest_movie_rtng AS (
    SELECT *
    FROM movie_rating
    WHERE avg_rating = (SELECT MAX(avg_rating) FROM movie_rating)
),

part_2_ans AS (
    SELECT MIN(title) AS results
    FROM highest_movie_rtng
)

SELECT * FROM part_1_ans
UNION ALL
SELECT * FROM part_2_ans;
