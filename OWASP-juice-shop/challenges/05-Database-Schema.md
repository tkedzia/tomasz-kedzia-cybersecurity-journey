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

# Coding Challenge: Database Schema (SQL Injection)

## Vulnerable Line Identification
**Line 5:**
```typescript
models.sequelize.query(`SELECT * FROM Products WHERE ((name LIKE '%${criteria}%' OR description LIKE '%${criteria}%') AND deletedAt IS NULL) ORDER BY name`)
```

---

## Step-by-Step Explanation

### Step 1: Input Extraction and Length Truncation
```typescript
let criteria: any = req.query.q === 'undefined' ? '' : req.query.q ?? ''
criteria = (criteria.length <= 200) ? criteria : criteria.substring(0, 200)
```
- **What happens:** The application grabs the search term `q` from the request query string and caps its length at 200 characters.
- **Why it is insecure:** Truncating string length does **not** sanitize or escape special SQL characters (like `'`, `"`, or `--`). An attacker can easily fit a full SQL injection payload inside 200 characters.

---

### Step 2: Unsafe Query Construction (SQL Injection)
```typescript
models.sequelize.query(`SELECT * FROM Products WHERE ((name LIKE '%${criteria}%' OR description LIKE '%${criteria}%') AND deletedAt IS NULL) ORDER BY name`)
```
- **What happens:** The variable `criteria` is injected directly into the SQL command string using JavaScript template literals (`${criteria}`).
- **The Vulnerability:** Because the database receives raw concatenated input, string literals inside `criteria` alter the structural SQL syntax. For example, passing `') OR '1'='1` breaks out of the `LIKE` condition and returns all records from the database.

---

### Step 3: Implementing the Correct Parameterized Fix
To eliminate SQL Injection in Sequelize raw queries:
1. Replace variable interpolations (`${criteria}`) with named parameters (`:criteria`).
2. Pass parameters via the `{ replacements: { criteria: ... } }` options object.
3. Pass the `%` wildcards **inside the replacement object**, not around the placeholder in the SQL string (placing `:criteria` inside string quotes like `'%:criteria%'` causes Sequelize to evaluate it as literal text rather than a bind variable).

---

## Final Version for GitHub Notes

```markdown
# Security Note: SQL Injection in Sequelize Raw Queries

## Overview
- **Vulnerability:** SQL Injection (OWASP A03:2021 – Injection)
- **CWE:** CWE-89 (Improper Neutralization of Special Elements used in an SQL Command)
- **Source File:** `searchProducts` handler

## Vulnerable Code
```typescript
// Line 5: Direct string interpolation into raw SQL query
models.sequelize.query(`SELECT * FROM Products WHERE ((name LIKE '%${criteria}%' OR description LIKE '%${criteria}%') AND deletedAt IS NULL) ORDER BY name`)
```

## Root Cause
User input from `req.query.q` is concatenated directly into the query string using template literals. Truncating input length to 200 characters does not sanitize malicious characters, allowing syntax manipulation and arbitrary SQL execution.

## Remediation / Complete Fixed Function
Use parameter replacements in Sequelize. Pass the wildcards (`%`) in the replacement value object to maintain strict boundary separation between code and data.

```typescript
export function searchProducts () {
  return (req: Request, res: Response, next: NextFunction) => {
    let criteria: any = req.query.q === 'undefined' ? '' : req.query.q ?? ''
    criteria = (criteria.length <= 200) ? criteria : criteria.substring(0, 200)

    // SAFE: Parameterized query using named replacements
    models.sequelize.query(
      `SELECT * FROM Products WHERE ((name LIKE :criteria OR description LIKE :criteria) AND deletedAt IS NULL) ORDER BY name`,
      { 
        replacements: { criteria: `%${criteria}%` } 
      }
    ).then(([products]: any) => {
        const dataString = JSON.stringify(products)
        for (let i = 0; i < products.length; i++) {
          products[i].name = req.__(products[i].name)
          products[i].description = req.__(products[i].description)
        }
        res.json(utils.queryResultToJson(products))
      }).catch((error: ErrorWithParent) => {
        next(error.parent)
      })
  }
}
```
```
