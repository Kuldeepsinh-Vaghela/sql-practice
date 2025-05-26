# LeetCode SQL: Patients With a Condition

**Link to the problem:**  
[https://leetcode.com/problems/patients-with-a-condition](https://leetcode.com/problems/patients-with-a-condition)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You are given a `Patients` table with columns: `patient_id`, `patient_name`, and `conditions`. The `conditions` column contains a space-separated list of medical conditions for each patient.

Your task is to find all patients who have been diagnosed with **DIAB1** (Diabetes Type 1). The condition name **may be anywhere in the string**, either at the start or after a space.

## SQL Solution

```sql
SELECT 
    patient_id, 
    patient_name, 
    conditions
FROM 
    patients
WHERE 
    conditions LIKE '% DIAB1%' 
    OR conditions LIKE 'DIAB1%';
