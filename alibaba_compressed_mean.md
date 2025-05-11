# 📦 Alibaba Compressed Mean

**Datalemur Question:** [Alibaba Compressed Mean](https://datalemur.com/questions/alibaba-compressed-mean)

---

## 📘 Problem Description

Alibaba has a table `items_per_order` that contains compressed data about orders: instead of storing one row per order, the data summarizes how many orders had a certain number of items.

Your task is to compute the **average number of items per order** (i.e., the mean), using the compressed format.

---

## 🧾 Table Schema

### items_per_order

| Column Name       | Type |
|-------------------|------|
| item_count        | int  |
| order_occurrences | int  |

- Each row tells you that there are `order_occurrences` orders that each contained `item_count` items.

---

## 📊 Sample Input

| item_count | order_occurrences |
|------------|-------------------|
| 1          | 3                 |
| 2          | 2                 |
| 3          | 1                 |

This means:
- 3 orders had 1 item each
- 2 orders had 2 items each
- 1 order had 3 items

---

## ✅ Expected Output

| mean |
|------|
| 1.7  |

Because:  
\[
\frac{(1×3 + 2×2 + 3×1)}{(3 + 2 + 1)} = \frac{3 + 4 + 3}{6} = \frac{10}{6} = 1.7
\]

---

## ✅ SQL Solution

```sql
SELECT 
  ROUND(SUM(item_count * order_occurrences)::numeric / SUM(order_occurrences), 1) AS mean
FROM items_per_order;
