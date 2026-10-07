## Challenge: Database Schema

### Objective
Exfiltrate the entire DB schema definition via SQL Injection.

### Solution
Send GET request to search endpoint >> `http://localhost:3000/rest/products/search?q=juice%25%27+AND+1%3C%3E1%29%29+UNION+ALL+SELECT+type%2C+name%2C+tbl_name%2C+rootpage%2C+sql%2C+7%2C+8%2C+9+%2C+10+FROM+sqlite_master--` >> Observe JSON response containing `sqlite_master` table structure >> SOLVED :)

### Relevant Concepts
* SQL Injection (SQLi)
* Database Schema Exfiltration
* UNION-Based SQLi

### Lessons Learned
Applications must never concatenate user input directly into SQL queries. Input parameters must be sanitized and bound using parameterized queries (prepared statements) to prevent attackers from injecting arbitrary SQL logic and reading database system metadata.

**SQL Injection & Schema Insights:**
* **What `sqlite_master` Is:** `sqlite_master` is a built-in system table in SQLite databases that stores the complete schema definition, including all table names, column structures, and index definitions (`sql` field).
* **Step-by-Step Query Execution Analysis:**
  1. **URL Decoding the Input:** The input parameter `q` is decoded by the server from `juice%27+AND+1%3C%3E1...` to `juice%' AND 1<>1)) UNION ALL SELECT type, name, tbl_name, rootpage, sql, 7, 8, 9, 10 FROM sqlite_master--`.
  2. **Breaking Out of the String Literal (`juice%'`):** The closing single quote `'` terminates the original search string literal inside the `LIKE '%<input>%'` clause, allowing subsequent text to be interpreted as SQL commands rather than plain text data.
  3. **Nullifying Original Query Results (`AND 1<>1))`):** The operator `1<>1` (1 is not equal to 1) evaluates to `FALSE`. Combining this with the closing double parentheses `))` balances the application's internal query grouping while forcing the original `Products` search to return zero rows. This leaves the result set clean so that only injected rows are returned.
  4. **Appending Schema Data (`UNION ALL SELECT ... FROM sqlite_master`):** The `UNION ALL` operator merges the results of the primary query with the secondary query. The secondary query selects schema metadata columns (`type`, `name`, `tbl_name`, `rootpage`, `sql`) along with dummy numeric constants (`7, 8, 9, 10`) to match the exact 9-column schema expected by the `Products` table.
  5. **Truncating Boilerplate Code (`--`):** The SQL comment sequence `--` instructs the database engine to ignore all remaining original query logic trailing after the input point (such as `%' OR description LIKE...`), preventing syntax errors due to unclosed quotes or parentheses.
