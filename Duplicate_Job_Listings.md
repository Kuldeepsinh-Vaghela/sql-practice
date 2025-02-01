# SQL Query: Identifying Companies with Duplicate Job Listings on LinkedIn

## Problem Statement
We are tasked with retrieving the count of companies that have posted **duplicate job listings** on LinkedIn. 

### Definition:
- **Duplicate job listings** are defined as two job listings within the same company that share **identical titles and descriptions**.

---

## Table: `job_listings`
| Column Name   | Type    |
|---------------|---------|
| job_id        | integer |
| company_id    | integer |
| title         | string  |
| description   | string  |

---

## Example Input:
| job_id | company_id | title          | description                                                                                                                                       |
|--------|------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| 248    | 827        | Business Analyst | Business analyst evaluates past and current business data with the primary goal of improving decision-making processes within organizations.    |
| 149    | 845        | Business Analyst | Business analyst evaluates past and current business data with the primary goal of improving decision-making processes within organizations.    |
| 945    | 345        | Data Analyst    | Data analyst reviews data to identify key insights into a business's customers and ways the data can be used to solve problems.                 |
| 164    | 345        | Data Analyst    | Data analyst reviews data to identify key insights into a business's customers and ways the data can be used to solve problems.                 |
| 172    | 244        | Data Engineer   | Data engineer works in a variety of settings to build systems that collect, manage, and convert raw data into usable information for data scientists and business analysts to interpret. |

---

## Example Output:
| duplicate_companies |
|---------------------|
| 1                   |

---

## Explanation:
- **Company 345** has posted two **duplicate job listings** (IDs 945 and 164) with identical titles ("Data Analyst") and descriptions. Therefore, the query returns **1** as the count of companies with duplicate listings.
- Other companies have no duplicate listings, so they are not counted.

---

## Additional Notes:
This query helps identify companies with potentially redundant or duplicate job postings. This could assist in cleaning up job listings and ensuring companies have unique entries for each role.

---

## SQL Query:
```sql
-- SELECT count(company_id) as duplicate_companies
-- FROM job_listings
-- except
-- select count(distinct(company_id))
-- from job_listings


select count(company_id) as duplicate_companies
from
(select company_id,title,  count(title) as job_count
from job_listings 
group by company_id, title
order by company_id) as sub_1
where job_count > 1;
