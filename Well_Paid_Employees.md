# SQL Query: Identify Employees Earning More Than Their Managers

## Problem Statement:
As an HR Analyst, you're tasked with identifying employees who earn more than their direct managers. The result should display the employee's **ID** and **name**.

---

## Table: `employee`

### Schema:
| Column Name    | Type   | Description                                    |
|----------------|--------|------------------------------------------------|
| employee_id    | integer | The unique ID of the employee.                 |
| name           | string  | The name of the employee.                      |
| salary         | integer | The salary of the employee.                    |
| department_id  | integer | The department ID of the employee.             |
| manager_id     | integer | The manager ID of the employee (nullable).     |

### Example `employee` Table Input:
| employee_id | name                | salary | department_id | manager_id |
|-------------|---------------------|--------|---------------|------------|
| 1           | Emma Thompson       | 3800   | 1             | 6          |
| 2           | Daniel Rodriguez    | 2230   | 1             | 7          |
| 3           | Olivia Smith        | 7000   | 1             | 8          |
| 4           | Noah Johnson        | 6800   | 2             | 9          |
| 5           | Sophia Martinez     | 1750   | 1             | 11         |
| 6           | Liam Brown          | 13000  | 3             | NULL       |
| 7           | Ava Garcia          | 12500  | 3             | NULL       |
| 8           | William Davis       | 6800   | 2             | NULL       |

---

## Example Output:
| employee_id | employee_name     |
|-------------|-------------------|
| 3           | Olivia Smith      |

---

## Explanation:
In this example, **Olivia Smith** earns **$7,000**, which is more than her manager, **William Davis**, who earns **$6,800**.

---

## Query Approach:
1. Join the `employee` table to itself by matching the `employee.manager_id` to the `employee.employee_id` (i.e., matching employees to their managers).
2. Compare the `salary` of employees with their respective managers.
3. Filter the result to only include employees whose salary is greater than their manager's salary.

---

## Notes:
- The `manager_id` can be `NULL`, indicating no manager for the employee.
- Ensure that the output contains only employees who earn more than their direct managers.

 ---

 ## SQL Query:
 ```sql 
SELECT 
  e1.employee_id,
  e1.name as employee_name
FROM employee e1 INNER JOIN employee e2
ON e1.manager_id = e2.employee_id
where e1.salary > e2.salary ;