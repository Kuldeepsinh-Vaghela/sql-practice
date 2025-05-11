# 💊 Top Profitable Drugs

**Datalemur Question:** [Top Profitable Drugs](https://datalemur.com/questions/top-profitable-drugs)

---

## 📘 Problem Description

You are given a table `pharmacy_sales` that contains the total sales and cost of goods sold (`cogs`) for different drugs.

Your task is to determine the **top 3 most profitable drugs**, where profit is calculated as:

profit = total_sales - cogs


Return the drug names and their corresponding total profits, ordered from highest to lowest.

---

## 🧾 Table Schema

### pharmacy_sales

| Column Name | Type    |
|-------------|---------|
| drug        | varchar |
| total_sales | int     |
| cogs        | int     |

- Each row represents one drug, its total revenue (`total_sales`), and cost of goods sold (`cogs`).

---

## 📊 Sample Input

| drug   | total_sales | cogs |
|--------|-------------|------|
| DrugA  | 1000        | 200  |
| DrugB  | 1500        | 700  |
| DrugC  | 500         | 100  |
| DrugD  | 300         | 50   |
| DrugE  | 800         | 300  |

---

## ✅ Expected Output

| drug   | total_profits |
|--------|----------------|
| DrugA  | 800            |
| DrugC  | 400            |
| DrugE  | 500            |

---

## ✅ SQL Solution

```sql
WITH drug_profits AS (
    SELECT 
        drug, 
        total_sales - cogs AS total_profits
    FROM pharmacy_sales
)

SELECT *
FROM drug_profits
ORDER BY total_profits DESC
LIMIT 3;
