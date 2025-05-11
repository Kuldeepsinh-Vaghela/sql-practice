# 🃏 Cards Issued Difference

**Datalemur Question:** [Cards Issued Difference](https://datalemur.com/questions/cards-issued-difference)

---

## 📘 Problem Description

You are given a table `monthly_cards_issued` that records the number of cards issued per card type (`card_name`) in each `issue_month`.

Your task is to calculate, for each card type, the **difference between the highest and lowest monthly issued amounts**. Return the result as a table with `card_name` and the `difference`, ordered by `difference` in descending order.

---

## 🧾 Table Schema

### monthly_cards_issued

| Column Name   | Type    |
|---------------|---------|
| card_name     | varchar |
| issued_amount | int     |
| issue_month   | date    |

- Each row represents the number of cards of a given `card_name` issued in a particular month.

---

## 📊 Sample Input

| card_name | issued_amount | issue_month |
|-----------|----------------|-------------|
| Chase     | 100            | 2021-01-01  |
| Chase     | 200            | 2021-02-01  |
| Chase     | 50             | 2021-03-01  |
| Amex      | 80             | 2021-01-01  |
| Amex      | 160            | 2021-02-01  |

---

## ✅ Expected Output

| card_name | difference |
|-----------|------------|
| Chase     | 150        |
| Amex      | 80         |

---

## ✅ SQL Solution

```sql
WITH monthly_card_amount AS (
    SELECT 
        card_name, 
        issue_month, 
        SUM(issued_amount) AS total_monthly_amount
    FROM monthly_cards_issued
    GROUP BY card_name, issue_month
),

ranked_monthly_card_amount AS (
    SELECT *, 
        RANK() OVER (PARTITION BY card_name ORDER BY total_monthly_amount) AS rank_no
    FROM monthly_card_amount
),

card_max_min_rank_no AS (
    SELECT 
        card_name, 
        MAX(rank_no) AS max_rank_no, 
        MIN(rank_no) AS min_rank_no
    FROM ranked_monthly_card_amount
    GROUP BY card_name
)

SELECT 
    ral.card_name, 
    ral.total_monthly_amount - rar.total_monthly_amount AS difference
FROM ranked_monthly_card_amount ral
JOIN ranked_monthly_card_amount rar
    ON ral.card_name = rar.card_name
LEFT JOIN card_max_min_rank_no cmn
    ON rar.card_name = cmn.card_name
WHERE ral.rank_no = cmn.max_rank_no 
  AND rar.rank_no = cmn.min_rank_no
ORDER BY difference DESC;
