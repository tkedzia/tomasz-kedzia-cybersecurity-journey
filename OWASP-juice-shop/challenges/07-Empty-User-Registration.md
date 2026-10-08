# Security Note: Zero-Length / Empty User Registration Bypass

## Overview
- **Vulnerability:** Improper Input Validation & Parameter Tampering (OWASP A04:2021 – Insecure Design / OWASP A03:2021 – Injection)
- **Target Endpoint:** `POST /api/Users/`
- **Tooling Used:** TamperDev (HTTP Interceptor & Request Manipulator)
- **Challenge Solved:** Empty User Registration (OWASP Juice Shop / Security CTF)

---

## Step-by-Step Execution

### Step 1: Request Interception Setup
1. Enabled HTTP request interception in **TamperDev**.
2. Triggered user registration within the application UI to capture the registration call.

### Step 2: Intercepting and Analyzing the Request
- TamperDev successfully paused the outgoing `POST` request:
  - **Method:** `POST`
  - **Host:** `localhost:3000`
  - **Path:** `/api/Users/`
  - **Status:** `intercepted`

### Step 3: Parameter Tampering / Payload Modification
1. Navigated to the **Request Body** editor inside TamperDev.
2. Cleared or set user fields (`email`, `password`, `securityAnswer`, etc.) to empty values or stripped mandatory payload attributes:
   ```json
   {
     "email": "",
     "password": "",
     "passwordRepeat": "",
     "securityQuestion": {
       "id": 1
     },
     "securityAnswer": ""
   }
   ```
3. Executed **Send response** / forwarded the modified `POST /api/Users/` request to the backend.

### Step 4: Verification
- The application backend accepted the empty strings or missing fields, returning an HTTP `201 Created` status code and creating an account with an empty identity string, completing the challenge.

---

## Technical Root Cause Analysis

1. **Missing Server-Side Validation:** The backend API relies on client-side form validation (HTML5 attributes / JS constraints) to enforce non-empty fields without re-validating the request body on the server.
2. **Insecure Entity Creation:** The database layer allows creation of user records where primary identify fields (such as `email`) are empty strings (`""`) or missing entirely.

---

## Remediation & Prevention

### 1. Robust Server-Side Schema Validation
Enforce strict input validation schemas on the server using libraries like `Joi`, `Yup`, or `express-validator`:

```typescript
import { body, validationResult } from 'express-validator'

export const validateUserRegistration = [
  body('email')
    .isEmail().withMessage('Must be a valid email address')
    .trim()
    .notEmpty().withMessage('Email cannot be empty'),
  body('password')
    .isLength({ min: 8 }).withMessage('Password must be at least 8 characters')
    .notEmpty().withMessage('Password cannot be empty'),
  (req: Request, res: Response, next: NextFunction) => {
    const errors = validationResult(req)
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() })
    }
    next()
  }
]
```

### 2. Database Constraints
Add `NOT NULL` constraints and non-empty checks at the database schema level:

```sql
ALTER TABLE Users 
ADD CONSTRAINT check_email_not_empty CHECK (length(trim(email)) > 0);
```
