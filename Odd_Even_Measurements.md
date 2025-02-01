# Measurement Summation Query

## Problem Statement:
You are tasked with calculating the sum of odd-numbered and even-numbered measurements separately for a particular day. The measurements are taken multiple times within each day, and you need to identify the sums for each type based on their order of occurrence (odd or even). The result should display these sums for each day.

### Definition:
- Odd-numbered measurements: These are the 1st, 3rd, 5th, etc., measurements taken within a day.
- Even-numbered measurements: These are the 2nd, 4th, 6th, etc., measurements taken within a day.
- The measurements should be grouped by **measurement_day** (the date part of `measurement_time`).

### Effective Date:
- The question and solution have been revised as of **April 15th, 2023**.

---

## Tables

### Table: `measurements`

| Column Name        | Type     | Description                               |
|--------------------|----------|-------------------------------------------|
| measurement_id     | integer  | Unique ID of the measurement.             |
| measurement_value  | decimal  | The value recorded by the sensor.         |
| measurement_time   | datetime | The exact date and time when the measurement was taken. |

#### Example Input for `measurements`:

| measurement_id | measurement_value | measurement_time        |
|----------------|-------------------|-------------------------|
| 131233         | 1109.51           | 07/10/2022 09:00:00     |
| 135211         | 1662.74           | 07/10/2022 11:00:00     |
| 523542         | 1246.24           | 07/10/2022 13:15:00     |
| 143562         | 1124.50           | 07/11/2022 15:00:00     |
| 346462         | 1234.14           | 07/11/2022 16:45:00     |

---

## Output Format

The output should display:

| measurement_day        | odd_sum  | even_sum |
|------------------------|----------|----------|
| 07/10/2022 00:00:00    | 2355.75  | 1662.74  |
| 07/11/2022 00:00:00    | 1124.50  | 1234.14  |

### Explanation:

- On **07/10/2022**:
  - **Odd-numbered measurements**: The 1st and 3rd measurements (1109.51 + 1246.24) give a total of **2355.75**.
  - **Even-numbered measurements**: The 2nd measurement (1662.74) gives a total of **1662.74**.

- On **07/11/2022**:
  - **Odd-numbered measurement**: The 1st measurement (1124.50) gives a total of **1124.50**.
  - **Even-numbered measurement**: The 2nd measurement (1234.14) gives a total of **1234.14**.

---

## Task:
Write a query to calculate the sum of the odd-numbered and even-numbered measurements for each day. The result should:
1. Be grouped by **measurement_day**.
2. Display the **odd_sum** and **even_sum** for each **measurement_day**.
3. Use the **measurement_time** to extract the day part (ignoring the time).
4. Ensure that the measurements are correctly classified as odd or even based on their order of occurrence within the day.

--- 

## Constraints:
- Ensure that the output shows the **measurement_day**, **odd_sum**, and **even_sum**.

---

## SQL Query:
```sql


with p1 as (select Date(measurement_time)as date_of_measurement, cast(measurement_time as time) time_of_day, measurement_value
from measurements),


p2 as (select p1.date_of_measurement, p1.time_of_day , measurement_value
from p1
group by p1.date_of_measurement, p1.time_of_day, measurement_value
order by p1.date_of_measurement),

p3 as (select p2.date_of_measurement, p2.time_of_day, p2.measurement_value,ROW_NUMBER() OVER (
PARTITION BY p2.date_of_measurement
order by p2.time_of_day asc ) row_num 
from p2),

p4 as (SELECT p3.date_of_measurement as measurement_day,sum(measurement_value) as odd_sum
from p3
where row_num in (1,3,5,7,9)
group by measurement_day),

p5 as (SELECT p3.date_of_measurement as measurement_day,sum(measurement_value) as even_sum
from p3
where row_num in (2,4,6,8,10)
group by measurement_day)

select p4.measurement_day, p4.odd_sum, p5.even_sum
from p4 natural join p5
order by p4.measurement_day;