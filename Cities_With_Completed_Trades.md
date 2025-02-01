# SQL Query: Retrieve Top 3 Cities with the Highest Number of Completed Trade Orders

## Problem Statement:
You are tasked with identifying the top three cities with the highest number of **completed trade orders** in the Robinhood trading system. The output should display the city name and the corresponding number of completed trade orders, listed in **descending order**.

---

## Tables

### `trades` Table:
| Column Name   | Type    |
|---------------|---------|
| order_id      | integer |
| user_id       | integer |
| quantity      | integer |
| status        | string  |
| date          | timestamp |
| price         | decimal (5, 2) |

### Example `trades` Table Input:
| order_id | user_id | quantity | status    | date                   | price |
|----------|---------|----------|-----------|------------------------|-------|
| 100101   | 111     | 10       | Cancelled | 08/17/2022 12:00:00    | 9.80  |
| 100102   | 111     | 10       | Completed | 08/17/2022 12:00:00    | 10.00 |
| 100259   | 148     | 35       | Completed | 08/25/2022 12:00:00    | 5.10  |
| 100264   | 148     | 40       | Completed | 08/26/2022 12:00:00    | 4.80  |
| 100305   | 300     | 15       | Completed | 09/05/2022 12:00:00    | 10.00 |
| 100400   | 178     | 32       | Completed | 09/17/2022 12:00:00    | 12.00 |
| 100565   | 265     | 2        | Completed | 09/27/2022 12:00:00    | 8.70  |

### `users` Table:
| Column Name   | Type    |
|---------------|---------|
| user_id       | integer |
| city          | string  |
| email         | string  |
| signup_date   | datetime |

### Example `users` Table Input:
| user_id | city          | email                          | signup_date          |
|---------|---------------|--------------------------------|----------------------|
| 111     | San Francisco | rrok10@gmail.com               | 08/03/2021 12:00:00  |
| 148     | Boston        | sailor9820@gmail.com           | 08/20/2021 12:00:00  |
| 178     | San Francisco | harrypotterfan182@gmail.com    | 01/05/2022 12:00:00  |
| 265     | Denver        | shadower_@hotmail.com          | 02/26/2022 12:00:00  |
| 300     | San Francisco | houstoncowboy1122@hotmail.com  | 06/30/2022 12:00:00  |

---

## Example Output:
| city          | total_orders |
|---------------|-------------|
| San Francisco | 3           |
| Boston        | 2           |
| Denver        | 1           |

---

## Explanation:
- **San Francisco** has the highest number of completed trade orders (3 orders).
- **Boston** holds the second position with 2 completed orders.
- **Denver** ranks third with 1 completed order.

The query will return the **top 3 cities** with the highest number of completed trade orders, sorted in descending order based on the count of completed orders.

---

## Notes:
- The trade order status must be "Completed" for it to be counted.
- Ensure the results are ordered by the number of completed orders in **descending order**.

---

## SQL Query:
```sql 
select city, count(order_id)
from trades, users
where trades.user_id = users.user_id
and status = 'Completed'
group by city
order by count(order_id) DESC
limit 3;