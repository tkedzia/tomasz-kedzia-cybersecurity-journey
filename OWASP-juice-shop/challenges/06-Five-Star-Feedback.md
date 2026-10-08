# Security Note: Admin Panel Exploitation & Feedback Deletion

## Overview
- **Vulnerability:** Broken Access Control / Functional-Level Access Control Failure (OWASP A01:2021) & SQL Injection / Authentication Bypass (OWASP A03:2021)
- **Target Route:** `http://localhost:3000/#/administration`
- **Challenge Solved:** 5-Star Feedback Deletion (OWASP Juice Shop / Security CTF)

---

## Step-by-Step Execution

### Step 1: Initial Compromise (Credential Theft & Cracking)
1. **Authentication Bypass via SQL Injection:**
   - Exploit a SQL Injection vulnerability in the login mechanism to retrieve the administrator hash. Reference writeup: [SQL Injection Lab Notes](https://github.com/tkedzia/tomasz-kedzia-cybersecurity-journey/blob/main/zaproxy-learning-lab/labs/lab-003-sql-injection/notes.md)
2. **Password Hash Cracking:**
   - Crack the extracted administrator password hash to gain valid plain-text credentials. Reference writeup: [Hashcat Password Cracking Lab](https://github.com/tkedzia/tomasz-kedzia-cybersecurity-journey/blob/main/hashcat-leraning-lab/01-Password-Hash-Cracking.md)

### Step 2: Accessing Administrative Capabilities
- Log in with the cracked administrator account.
- Navigate directly to the administrative route:
  ```text
  http://localhost:3000/#/administration
  ```
- Access administrative functions, including the feedback and review management table.

### Step 3: Deleting the Target Feedback
- Locate the entry containing the target **5-star customer feedback**.
- Click the delete action associated with the entry, triggering an HTTP request to the backend:
  ```http
  DELETE /api/Feedbacks/{id} HTTP/1.1
  Host: localhost:3000
  Authorization: Bearer <token>
  ```
- Confirm successful removal to complete the objective.

---

## Technical Root Cause Analysis

1. **Unsanitized SQL Input:** Authentication routines concatenated untrusted input directly into SQL queries, leading to credential extraction/bypass.
2. **Client-Side Route Exposure & Weak Authorization:** The application relied on client-side controls to obscure administrative routes without enforcing robust server-side permission checks on endpoints such as `DELETE /api/Feedbacks/:id`.

---

## Remediation & Security Controls

### 1. Parameterized Queries (Prevent SQLi)
Use bound parameters or an ORM to prevent SQL Injection during authentication:

```typescript
// Safe SQL Query using Parameterized Input
const user = await sequelize.query(
  'SELECT * FROM Users WHERE email = :email AND password = :password',
  {
    replacements: { email, password },
    type: QueryTypes.SELECT
  }
)
```

### 2. Server-Side Role Middleware (Prevent Access Control Failure)
Always validate user roles on the server for sensitive endpoints:

```typescript
// Express.js Authorization Middleware Example
export function requireAdmin(req: Request, res: Response, next: NextFunction) {
  if (req.user && req.user.role === 'admin') {
    return next()
  }
  return res.status(403).json({ status: 'error', message: 'Access denied: Admin privileges required.' })
}

// Protected Endpoint Route
app.delete('/api/Feedbacks/:id', requireAdmin, deleteFeedbackController)
```
