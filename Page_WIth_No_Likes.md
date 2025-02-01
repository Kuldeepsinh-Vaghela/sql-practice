# SQL Query: Finding Facebook Pages with Zero Likes

## Problem Statement
Given two tables, one containing data about Facebook Pages (`pages` table) and another containing data about users who have liked those pages (`page_likes` table), write a query to return the IDs of the Facebook pages that have **zero likes**. The output should be sorted in ascending order based on the page IDs.

---

## Tables:

### Table: `pages`
| Column Name | Type      |
|-------------|-----------|
| page_id     | integer   |
| page_name   | varchar   |

**Example Input:**
| page_id | page_name          |
|---------|--------------------|
| 20001   | SQL Solutions      |
| 20045   | Brain Exercises    |
| 20701   | Tips for Data Analysts |

---

### Table: `page_likes`
| Column Name | Type      |
|-------------|-----------|
| user_id     | integer   |
| page_id     | integer   |
| liked_date  | datetime  |

**Example Input:**
| user_id | page_id | liked_date           |
|---------|---------|----------------------|
| 111     | 20001   | 04/08/2022 00:00:00  |
| 121     | 20045   | 03/12/2022 00:00:00  |
| 156     | 20001   | 07/25/2022 00:00:00  |

---
## Example Output:
|page_id|
|-------|
|20701|

----

## SQL Query:
```sql
SELECT page_id FROM pages
Except
SELECT page_id FROM page_likes


