# SQL Query: Identifying Unfinished Parts in Production

## Problem Statement
Tesla is investigating production bottlenecks and needs help to extract relevant data. The goal is to determine which parts have begun the assembly process but are not yet finished.

### Assumptions:
- The `parts_assembly` table contains all parts currently in production, each at varying stages of the assembly process.
- An **unfinished part** is one that **lacks a finish date**.
- The parts that have started the assembly process but are not finished are identified by missing `finish_date` values.

---

## Table: `parts_assembly`
| Column Name    | Type      |
|----------------|-----------|
| part           | string    |
| finish_date    | datetime  |
| assembly_step  | integer   |

---

## Example Input:
| part     | finish_date           | assembly_step |
|----------|-----------------------|---------------|
| battery  | 01/22/2022 00:00:00   | 1             |
| battery  | 02/22/2022 00:00:00   | 2             |
| battery  | 03/22/2022 00:00:00   | 3             |
| bumper   | 01/22/2022 00:00:00   | 1             |
| bumper   | 02/22/2022 00:00:00   | 2             |
| bumper   | NULL                  | 3             |
| bumper   | NULL                  | 4             |

---

## Example Output:
| part     | assembly_step |
|----------|---------------|
| bumper   | 3             |
| bumper   | 4             |

---

## Explanation:
- The **bumpers** are still in assembly steps 3 and 4 because their `finish_date` is missing, indicating they are unfinished.
- The **batteries** are not included in the output because all their assembly steps are completed with recorded `finish_date`s.

---

## Additional Notes:
This query helps Tesla identify the parts that have entered the assembly process but have not yet finished, which can be crucial for understanding production bottlenecks and improving efficiency.

---

## SQL Query:
```sql
SELECT part, assembly_step FROM parts_assembly
where finish_date is null;
