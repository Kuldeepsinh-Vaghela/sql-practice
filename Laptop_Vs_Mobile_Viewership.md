# SQL Query: Calculating Total Viewership for Laptops and Mobile Devices

## Problem Statement
We are tasked with calculating the total viewership for laptops and mobile devices. Here, "mobile" is defined as the sum of tablet and phone viewership.

### Assumptions:
- The `viewership` table contains data on user viewership categorized by device type.
- The device types are:
  - `laptop`
  - `tablet`
  - `phone`

---

## Table: `viewership`
| Column Name  | Type      |
|--------------|-----------|
| user_id      | integer   |
| device_type  | string    |
| view_time    | timestamp |

---

## Example Input:
| user_id | device_type | view_time             |
|---------|-------------|-----------------------|
| 123     | tablet      | 01/02/2022 00:00:00   |
| 125     | laptop      | 01/07/2022 00:00:00   |
| 128     | laptop      | 02/09/2022 00:00:00   |
| 129     | phone       | 02/09/2022 00:00:00   |
| 145     | tablet      | 02/24/2022 00:00:00   |

---

## Example Output:
| laptop_views | mobile_views |
|--------------|--------------|
| 2            | 3            |

---

## Explanation:
- **Laptop Views (`laptop_views`)**: There are 2 views from devices classified as `laptop` (user IDs 125 and 128).
- **Mobile Views (`mobile_views`)**: The total mobile viewership is the sum of `tablet` and `phone` viewership, which gives 3 views (user IDs 123, 145 for tablet, and 129 for phone).

---

## Additional Notes:
This query is useful for analyzing the total viewership for different types of devices, especially when wanting to group mobile devices (tablet + phone) as a single category. The calculation helps in comparing laptop and mobile views more easily.

---

## SQL Query:
```sql
select 
sum(case when device_type = 'laptop' then 1 else 0 end) as laptop_views,
sum(case when device_type in ('tablet','phone') then 1 else 0 end) as mobile_views
from viewership;