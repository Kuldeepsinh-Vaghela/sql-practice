# SQL Query: Average Star Rating for Each Product Grouped by Month

## Problem Statement:
Write a query to retrieve the **average star rating** for each product, **grouped by month**. The output should display the **month** as a numerical value, **product ID**, and the **average star rating** rounded to two decimal places. The results should be sorted first by month and then by product ID.

---

## Tables

### `reviews` Table:
| Column Name   | Type    |
|---------------|---------|
| review_id     | integer |
| user_id       | integer |
| submit_date   | datetime |
| product_id    | integer |
| stars         | integer (1-5) |

### Example `reviews` Table Input:
| review_id | user_id | submit_date           | product_id | stars |
|-----------|---------|-----------------------|------------|-------|
| 6171      | 123     | 06/08/2022 00:00:00   | 50001      | 4     |
| 7802      | 265     | 06/10/2022 00:00:00   | 69852      | 4     |
| 5293      | 362     | 06/18/2022 00:00:00   | 50001      | 3     |
| 6352      | 192     | 07/26/2022 00:00:00   | 69852      | 3     |
| 4517      | 981     | 07/05/2022 00:00:00   | 69852      | 2     |

---

## Example Output:
| mth | product | avg_stars |
|-----|---------|-----------|
| 6   | 50001   | 3.50      |
| 6   | 69852   | 4.00      |
| 7   | 69852   | 2.50      |

---

## Explanation:
- **Product 50001** received two ratings of 4 and 3 in the month of June (6th month), resulting in an average star rating of **3.5**.
- **Product 69852** received two ratings of 4 and 3 in June, and two ratings of 2 and 3 in July. The average rating for June is **4.0**, and for July is **2.5**.

---

## Notes:
- Round the **average star rating** to two decimal places.
- Group the data by **month** and **product_id**.
- Sort the result first by **month**, then by **product_id**.

---

## SQL Query:
```sql
SELECT extract(MONTH from submit_date) as mth, product_id as product,round(avg(stars),2) as avg_stars
FROM reviews
group by mth, product
order by mth, product;