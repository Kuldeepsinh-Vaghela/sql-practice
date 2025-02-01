# SQL Query: Identifying the Top 2 Power Users Based on Message Count in August 2022

## Problem Statement
We are tasked with identifying the **top 2 Power Users** who sent the highest number of messages on **Microsoft Teams** in **August 2022**. The result should display the **sender_id** of these two users along with the **total number of messages** they sent, ordered by message count in descending order.

### Assumptions:
- No two users have sent the same number of messages in **August 2022**.
- The query should return the **top 2 users** based on message count.

---

## Table: `messages`
| Column Name   | Type      |
|---------------|-----------|
| message_id    | integer   |
| sender_id     | integer   |
| receiver_id   | integer   |
| content       | varchar   |
| sent_date     | datetime  |

---

## Example Input:
| message_id | sender_id | receiver_id | content                     | sent_date             |
|------------|-----------|-------------|-----------------------------|-----------------------|
| 901        | 3601      | 4500        | You up?                     | 08/03/2022 00:00:00   |
| 902        | 4500      | 3601        | Only if you're buying        | 08/03/2022 00:00:00   |
| 743        | 3601      | 8752        | Let's take this offline      | 06/14/2022 00:00:00   |
| 922        | 3601      | 4500        | Get on the call              | 08/10/2022 00:00:00   |

---

## Example Output:
| sender_id | message_count |
|-----------|---------------|
| 3601      | 2             |
| 4500      | 1             |

---

## Explanation:
- **Sender 3601** has sent **2 messages** in August 2022 (both messages were exchanged with sender 4500).
- **Sender 4500** has sent **1 message** in August 2022 (to sender 3601).
- The query returns the top 2 users ordered by the number of messages they sent, in descending order.

---

## Additional Notes:
This query is useful to identify the most active users on Microsoft Teams within a specific period, in this case, **August 2022**. The results could help in understanding user engagement and communication patterns within an organization.

---

## SQL Query
```sql
SELECT sender_id, count(message_id)as message_count
FROM messages
where sent_date between '08/01/2022' and '09/01/2022'
group by sender_id
order by message_count desc
limit 2;