# **SQL Injection: Authentication Bypass & UNION Attack**

---

### **Overview**
This lab demonstrates how an application vulnerability allows an attacker to log in as a non-existing user (such as an accountant) by exploiting an SQL Injection (SQLi) flaw in the login functionality, without ever registering that user.

---

### **1. Reconnaissance & Error Observation**

* **Target URL:** `http://localhost:3000/#/login`
* **Action:** Attempt to log in using a single quote (`'`) in the Email field and any arbitrary password.
* **Observation:** Open the **Browser DevTools Network Tab** and inspect the error response returned by the server. You will see the raw SQL query exposed in the error message:

```json
{
  "sql": "SELECT * FROM Users WHERE email = ''' AND password = '339df5aeae5bc6ae557491e02619c5dd' AND deletedAt IS NULL"
}
