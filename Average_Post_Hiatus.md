# SQL Query: Calculating Days Between First and Last Post of the Year for Active Users

## Problem Statement
We need to find the number of days between each user’s first post and last post of the year 2021, but only for users who posted at least twice in 2021.

### Assumptions:
- The `posts` table contains data on Facebook posts, with details including user ID, post ID, content, and the post date.
- We need to consider only users who posted **at least twice** in 2021.
- The goal is to calculate the number of days between the first and last post of each user in 2021.

---

## Table: `posts`
| Column Name   | Type      |
|---------------|-----------|
| user_id       | integer   |
| post_id       | integer   |
| post_content  | text      |
| post_date     | timestamp |

---

## Example Input:
| user_id | post_id | post_content                                           | post_date             |
|---------|---------|--------------------------------------------------------|-----------------------|
| 151652  | 599415  | Need a hug                                             | 07/10/2021 12:00:00   |
| 661093  | 624356  | Bed. Class 8-12. Work 12-3. Gym 3-5 or 6. Then class 6-10. Another day that's gonna fly by. I miss my girlfriend | 07/29/2021 13:00:00   |
| 004239  | 784254  | Happy 4th of July!                                     | 07/04/2021 11:00:00   |
| 661093  | 442560  | Just going to cry myself to sleep after watching Marley and Me. | 07/08/2021 14:00:00   |
| 151652  | 111766  | I'm so done with covid - need travelling ASAP!         | 07/12/2021 19:00:00   |

---

## Example Output:
| user_id | days_between |
|---------|--------------|
| 151652  | 2            |
| 661093  | 21           |

---

## Explanation:
- **User 151652**: Their first post in 2021 was on **July 10** and their last post was on **July 12**, which gives a difference of **2 days**.
- **User 661093**: Their first post was on **July 8** and their last post was on **July 29**, giving a difference of **21 days**.

---

## Additional Notes:
This query helps identify the activity span of users who posted multiple times in a year, and could be useful in understanding user engagement trends for a specific year.

---

## SQL Query:
```sql
select user_id, days_between
from (SELECT user_id, extract(DAY from (max(post_date) - min(post_date))) as days_between
FROM posts
where post_date between '01/01/2021 00:00:00' and '12/31/2021 24:00:00' 
group by user_id
order by user_id) as sub_1
where days_between > 0;