# OWASP Juice Shop — Login Bender via SQL Injection

> **Scope:** Self-hosted/local OWASP Juice Shop training environment only  
> **Category:** Injection  
> **Challenge:** Login Bender (the same applies to Login Jim challange)
> **Objective:** Log in using Bender’s account without knowing or cracking the password.

## Summary

I solved the “Login Bender” challenge by intercepting and modifying the login request with OWASP ZAP. The application accepted user-controlled input from the `email` field and inserted it directly into a dynamically constructed SQL query. This made it possible to alter the query logic and comment out the password-verification condition.

The successful request returned HTTP `200 OK` and an authentication token, confirming that the application authenticated the session as Bender. The Juice Shop challenge banner also confirmed completion: “Login Bender (Log in with Bender’s user account.)”.

## Tools Used

- OWASP Juice Shop — intentionally vulnerable training application
- OWASP ZAP — intercepting proxy and HTTP request editor, steps performed (in short: performing manual explore using ZAProxy, attempted login request with valid email and random password, found the request recorded by ZAP on the page tree, mause right clisk and selected Open/.Resend with Request Editor, altered the request as described below, re-sent it, SOLVED): https://drive.google.com/file/d/1WZtCVh0kPKA-OAdtbJDZ-9y4yQXz0-M2/view?usp=drive_link
- Browser launched/configured through ZAP

## Target Endpoint

```http
POST /rest/user/login
```

## Modified Request Body

```json
{
  "email": "bender@juice-sh.op'-- ",
  "password": "x"
}
```

## Vulnerability

The vulnerable login implementation constructed a SQL query by interpolating request data directly into the query string:

```ts
SELECT * FROM Users
WHERE email = '${req.body.email || ''}'
  AND password = '${security.hash(req.body.password || '')}'
```

The problem was the direct use of:

```ts
req.body.email
```

inside the SQL statement. This allowed special SQL characters supplied in the `email` field to change the intended query structure.

## Exploitation Logic

The supplied e-mail value was:

```text
bender@juice-sh.op'-- 
```

Its elements have the following effect:

1. `bender@juice-sh.op` targets Bender’s user record.
2. The apostrophe (`'`) closes the SQL string literal surrounding the e-mail address.
3. `-- ` begins an SQL comment. The trailing space was included because some SQL parsers require whitespace after `--`.
4. The remaining generated SQL, including the password comparison, is treated as a comment.

Conceptually, the application produced a query similar to:

```sql
SELECT * FROM Users
WHERE email = 'bender@juice-sh.op'-- '
  AND password = '<hashed value>';
```

The database therefore effectively evaluated only:

```sql
SELECT * FROM Users
WHERE email = 'bender@juice-sh.op'
```

As a result, the application returned Bender’s user record without validating the supplied password value. The value `x` in the password field was arbitrary and was not used in the final authentication decision.

## Evidence of Success

The modified request resulted in:

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

The response included an `authentication` object containing a token, and Juice Shop displayed the successful challenge notification. The token is intentionally not included in this repository because authentication tokens should be treated as sensitive session material, even in a laboratory environment.

## Security Impact

A real application affected by this vulnerability could allow an attacker to:

- bypass password authentication;
- access arbitrary user accounts by knowing or guessing an e-mail address;
- access data and functions available to the impersonated user;
- escalate impact if an administrative or privileged account is targeted.

The vulnerability is especially severe because it enables account takeover without password cracking, credential stuffing, or user interaction.

## Root Cause

The root cause was dynamic SQL construction through string interpolation. User-controlled data was combined with SQL syntax rather than being bound as data parameters.

Hashing the submitted password did not prevent the attack. The injection occurred through the e-mail input before the password check could be meaningfully enforced.

## Remediation

Use parameterized queries or prepared statements. User input must be passed separately from the SQL query structure.

Example using Sequelize replacements:

```ts
models.sequelize.query(
  `SELECT * FROM Users
   WHERE email = :email
     AND password = :password`,
  {
    replacements: {
      email: req.body.email ?? '',
      password: security.hash(req.body.password ?? '')
    }
  }
)
```

Additional defensive measures:

- validate e-mail format as a secondary control;
- apply least-privilege permissions to the database account used by the application;
- log and monitor failed or anomalous login attempts;
- use a modern password-hashing algorithm such as Argon2id, bcrypt, or scrypt with a unique salt;
- implement MFA for privileged and high-risk accounts;
- perform secure code review and dynamic testing for injection flaws.

## Key Takeaway

SQL injection is not only a data-extraction vulnerability. When it affects authentication logic, it can enable direct authentication bypass. Parameterized queries prevent the database from interpreting input values as SQL syntax, separating user data from executable query structure.
