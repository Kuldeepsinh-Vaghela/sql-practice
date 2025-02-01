# SQL Query: Finding the Best Candidates for a Data Science Job

## Problem Statement
Given a table of candidates and their skills, find candidates who are proficient in **Python, Tableau, and PostgreSQL**. The output should list **only those candidates** who possess **all** of the required skills. The results should be sorted by `candidate_id` in ascending order.

---

## Table: `candidates`
| Column Name   | Type    |
|--------------|---------|
| candidate_id | integer |
| skill        | varchar |

---

## Example Input:
| candidate_id | skill       |
|-------------|------------|
| 123         | Python      |
| 123         | Tableau     |
| 123         | PostgreSQL  |
| 234         | R           |
| 234         | PowerBI     |
| 234         | SQL Server  |
| 345         | Python      |
| 345         | Tableau     |

---

## Example Output:
| candidate_id | 
|-------------|
| 123         | 

---

## Explanation
- **Candidate `123`** is displayed because they have **all** the required skills:  
  ✅ Python  
  ✅ Tableau  
  ✅ PostgreSQL  

- **Candidate `345`** is **not included** because they are missing one of the required skills:  
  ❌ PostgreSQL (missing)  

---

## SQL Query:
```sql
SELECT candidate_id
FROM candidates
WHERE skill IN ('Python','Tableau', 'PostgreSQL')
GROUP BY candidate_id
HAVING count(skill) = 3
ORDER BY candidate_id;
