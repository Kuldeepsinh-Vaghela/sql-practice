# LeetCode SQL: Find Users With Valid E-Mails

**Link to the problem:**  
[https://leetcode.com/problems/find-users-with-valid-e-mails](https://leetcode.com/problems/find-users-with-valid-e-mails)

## Problem Description

This SQL problem is part of the [Top SQL 50](https://leetcode.com/study-plan/top-sql-50/) study plan on LeetCode.

You're given a `Users` table with a `mail` column. Your task is to return all users whose email address is **valid** according to the following rules:

1. The email must end with `@leetcode.com`
2. The local part (before the `@`) must:
   - Start with at least one English letter `[a-zA-Z]`
   - Be followed by any number of letters, digits, hyphens (`-`), underscores (`_`), or periods (`.`)

## SQL Solution

```sql
SELECT *
FROM users
WHERE mail RLIKE '^[a-zA-Z]+[a-zA-Z0-9-_.]*@leetcode\\.com$';
