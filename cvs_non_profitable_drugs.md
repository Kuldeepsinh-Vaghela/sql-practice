# 💸 Non-Profitable Drugs

**Datalemur Question:** [Non-Profitable Drugs](https://datalemur.com/questions/non-profitable-drugs)

---

## 📘 Problem Description

You are given a table `pharmacy_sales` that contains data on drugs, their manufacturers, total sales, and cost of goods sold (`cogs`).

Your task is to determine which manufacturers have **non-profitable drugs** — where a drug's **profit is negative** (`total_sales < cogs`).

For each such manufacturer:
- Count how many non-profitable drugs they have.
- Calculate the total amount of loss from those drugs (i.e., the sum of negative profits, expressed as positive numbers).

Return a result table with:
- `manufacturer`
- `drug_count`: number of non-profitable drugs
- `total_loss`: total loss incurred across those drugs

Sort the output by `drug_count` in descending order.

---

## 🧾 Table Schema

### pharmacy_sales

| Column Name | Type    |
|-------------|---------|
| manufacturer| varchar |
| drug        | varchar |
| total_sales | int     |
| cogs        | int     |

- Each row represents a drug, its manufacturer, revenue (`total_sales`), and cost of goods sold (`cogs`).

---

## 📊 Sample Input

| manufacturer | drug   | total_sales | cogs |
|--------------|--------|-------------|------|
| M1           | DrugA  | 100         | 150  |
| M1           | DrugB  | 300         | 200  |
| M2           | DrugC  | 400         | 500  |
| M2           | DrugD  | 100         | 50   |
| M3           | DrugE  | 200         | 300  |

---

## ✅ Expected Output

| manufacturer | drug_count | total_loss |
|--------------|------------|------------|
| M1           | 1          | 50         |
| M2           | 1          | 100        |
| M3           | 1          | 100        |

---

## ✅ SQL Solution

```sql
WITH manu_drug_pl AS (
    SELECT 
        manufacturer, 
        drug, 
        total_sales - cogs AS total_pl
    FROM pharmacy_sales
),

manu_loss AS (
    SELECT *,
        CASE 
            WHEN total_pl < 0 THEN total_pl * -1 
            ELSE 0 
        END AS loss_value
    FROM manu_drug_pl
),

manu_drug_count AS (
    SELECT 
        manufacturer, 
        COUNT(*) AS drug_count
    FROM manu_loss
    WHERE total_pl < 0
    GROUP BY manufacturer
),

manu_loss_count AS (
    SELECT 
        manufacturer, 
        SUM(loss_value) AS total_loss
    FROM manu_loss
    WHERE total_pl < 0
    GROUP BY manufacturer
)

SELECT 
    mdc.manufacturer, 
    mdc.drug_count, 
    mlc.total_loss
FROM manu_drug_count mdc
INNER JOIN manu_loss_count mlc
    ON mdc.manufacturer = mlc.manufacturer
ORDER BY mdc.drug_count DESC;
