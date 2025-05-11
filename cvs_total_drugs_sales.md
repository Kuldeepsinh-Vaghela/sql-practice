# 💰 Total Drugs Sales

**Datalemur Question:** [Total Drugs Sales](https://datalemur.com/questions/total-drugs-sales)

---

## 📘 Problem Description

You are given a table `pharmacy_sales` that contains drug sales data. Your task is to **compute the total sales per manufacturer**, expressed in **millions of dollars**, and return a formatted result.

The result should:
- Show each `manufacturer`
- Show their **total sales**, rounded to the nearest million, and
- Format the result as a string like: `"$6 million"`

Sort the result by:
1. Descending total sales
2. Descending manufacturer name

---

## 🧾 Table Schema

### pharmacy_sales

| Column Name   | Type    |
|---------------|---------|
| manufacturer  | varchar |
| drug          | varchar |
| total_sales   | int     |
| cogs          | int     |

- Each row represents the sales and cost information for a single drug and its manufacturer.

---

## 📊 Sample Input

| manufacturer | drug   | total_sales | cogs |
|--------------|--------|-------------|------|
| M1           | DrugA  | 2000000     | 1000000 |
| M2           | DrugB  | 5000000     | 3000000 |
| M1           | DrugC  | 4000000     | 2000000 |
| M3           | DrugD  | 1000000     | 500000  |

---

## ✅ Expected Output

| manufacturer | sales_mil     |
|--------------|----------------|
| M1           | $6 million     |
| M2           | $5 million     |
| M3           | $1 million     |

---

## ✅ SQL Solution

```sql
WITH manu_tot_sale AS (
    SELECT 
        manufacturer,
        ROUND(SUM(total_sales) / 1000000) AS tot_sales
    FROM pharmacy_sales
    GROUP BY manufacturer
    ORDER BY tot_sales DESC, manufacturer DESC
)

SELECT 
    manufacturer, 
    CONCAT('$' || tot_sales || ' million') AS sales_mil
FROM manu_tot_sale;
