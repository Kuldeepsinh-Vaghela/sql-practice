# LeetCode SQL: Second Highest Salary

**Problem Link:**  
[https://leetcode.com/problems/second-highest-salary](https://leetcode.com/problems/second-highest-salary/?envType=study-plan-v2&envId=top-sql-50)

---

## 🧩 Problem Description

Write a SQL query to get the second highest salary from the `Employee` table. If there is no second highest salary, the result should be `null`.

### `Employee` Table  
| Column  | Type    |
|---------|---------|
| id      | int     |
| salary  | int     |

---

## ✅ Solution Using Subquery and MAX

```sql
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary < (
    SELECT MAX(salary)
    FROM Employee
);
