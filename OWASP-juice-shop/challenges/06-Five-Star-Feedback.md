# Security Note: Admin Panel Exploitation & Feedback Deletion

## Overview
- **Vulnerability:** Broken Access Control / Functional-Level Access Control Failure (OWASP A01:2021)
- **Target Route:** `http://localhost:3000/#/administration`
- **Challenge Solved:** 5-Star Feedback Deletion (OWASP Juice Shop / Security CTF)

---

## Step-by-Step Execution

### Step 1: Endpoint Discovery
- Located the hidden or previously enumerated administrative route:
  ```text
  http://localhost:3000/#/administration
  ```

### Step 2: Accessing Administrative Capabilities
- Navigated directly to the `# /administration` client-side route.
- Accessed administrative functions, including the feedback and review management view.

### Step 3: Deleting the Target Feedback
- Located the entry containing the target **5-star customer feedback**.
- Clicked the delete action associated with the entry, triggering an HTTP request to the backend:
  ```http
  DELETE /api/Feedbacks/{id} HTTP/1.1
  Host: localhost:3000
  Authorization: Bearer <token>
  ```
- Confirmed successful removal, solving the challenge.

---

## Technical Root Cause Analysis

1. **Client-Side Route Exposure:** The application relies on client-side router guards or hidden links to obscure administrative pages rather than enforcing strict server-side access control.
2. **Missing Inbound Authorization:** Backend endpoints (e.g., `DELETE /api/Feedbacks/:id`) fail to verify whether the requesting user's JWT or session possesses valid administrator permissions (`role === 'admin'`).

---

## Remediation & Security Controls

### 1. Server-Side Role Middleware (Primary Control)
Always validate authorization state on the backend for every privileged endpoint:

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

### 2. Client-Side Navigation Guards
Prevent rendering of sensitive view components for non-administrative roles:

```typescript
// SPA Router Guard Pattern
if (!authService.isLoggedIn() || !authService.hasRole('admin')) {
  router.navigate(['/403'])
}
```
