# LeetCode SQL: Department Top Three Salaries

**Problem Link:**  
[https://leetcode.com/problems/department-top-three-salaries](https://leetcode.com/problems/department-top-three-salaries/?envType=study-plan-v2&envId=top-sql-50)

---

## 🧩 Problem Description

Given two tables: `Employee` and `Department`, return the top **three highest salaries** for each department.

### `Employee` Table  
| Column        | Type    |
|---------------|---------|
| id            | int     |
| name          | varchar |
| salary        | int     |
| departmentId  | int     |

### `Department` Table  
| Column | Type    |
|--------|---------|
| id     | int     |
| name   | varchar |

---

## ✅ Solution Using CTE and Dense Rank

```sql
WITH dept_rank AS (
    SELECT 
        departmentId,
        e.name AS employee,
        d.name AS department,
        salary,
        DENSE_RANK() OVER (
            PARTITION BY departmentId, d.name 
            ORDER BY salary DESC
        ) AS rank_val
    FROM employee e
    INNER JOIN department d
        ON e.departmentId = d.id
)

SELECT department, employee, salary
FROM dept_rank
WHERE rank_val <= 3
ORDER BY salary DESC;
