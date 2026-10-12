# OWASP Juice Shop — Client-side XSS Protection

> Scope: Self-hosted/local OWASP Juice Shop training environment only  
> Category: Cross-Site Scripting (XSS)  
> Technique: Stored XSS through a direct API request  
> Tool: OWASP ZAP — Requester  
> Challenge: Client-side XSS Protection  
> Date: 12 October 2026  
> Objective: Submit a persisted XSS payload while bypassing a client-side security mechanism.

## Executive summary

I completed the “Client-side XSS Protection” challenge in my local OWASP Juice Shop training environment.

Instead of submitting data through the normal registration interface, I used OWASP ZAP to send a request directly to the user-creation API.

The request placed an HTML/JavaScript payload into the `email` field. The server accepted the request, created a customer account, and returned the payload in the account data.

When I subsequently viewed the administration page, the browser displayed an alert containing `xss`. This demonstrated that the injected input was executed rather than remaining harmless text.

This was **stored XSS**: the input was saved as account data and later reached a page where it could execute.

The central lesson is that browser-side protections cannot be the only security boundary. An attacker can bypass the normal interface and communicate directly with the backend.

## Environment and scope

| Item | Value |
|---|---|
| Application | OWASP Juice Shop |
| Target | `http://localhost:3000` |
| Scope | Local/self-hosted training environment |
| Tool | OWASP ZAP Requester |
| Submission endpoint | `POST /api/Users` |
| Request format | JSON |
| Injection field | `email` |
| Created user ID | `27` |
| Returned account role | `customer` |
| Execution page | `http://localhost:3000/#/administration` |
| Proof of execution | Browser alert displaying `xss` |

This was hands-on laboratory practice against an intentionally vulnerable application, not testing of a production system.

The screenshots do not identify the authenticated account used to view the administration page. I therefore do not attribute that browsing session to a particular user.

## What the application should normally do

A registration form should accept an email address as account data.

For example:

```json
{
  "email": "test@example.com",
  "password": "<test-password>"
}
```

Two separate safeguards are important:

1. The backend should validate that the submitted value meets the application's rules for an email address.
2. Every page displaying that value should treat it as text, not as executable HTML or JavaScript.

The second safeguard matters even when validation exists. A display component must remain safe when it receives unexpected data.

## Explanation for a non-technical reader

Imagine a company registration form asking for an email address.

Someone writes an instruction in the email box instead of an address:

> “When this record is displayed, run this command.”

A secure system stores or rejects that input as appropriate, but never treats it as an instruction to the person or software reading it.

In this exercise, the website accepted the instruction as account data. Later, when the administration page displayed the user information, the browser interpreted part of that data as something to execute.

The result was a popup saying `xss`.

The popup itself was harmless. Its importance is that it proved a boundary had been crossed:

```text
User-controlled data → executable browser behavior
```

### Why the normal form did not stop the test

A browser form is only one way to communicate with a web application.

The normal interface may check input before submitting it. However, a testing tool can send an HTTP request directly to the same backend endpoint without using that form.

A useful analogy is a reception desk that checks paperwork while another entrance accepts it without the same checks.

The backend must enforce its own rules because the client — whether a browser, script, or testing tool — is not a trusted security boundary.

In this exercise, I bypassed the normal registration interface by submitting the request through OWASP ZAP.

The screenshots show the direct API request. They do not show a separate test of the registration form's exact validation behavior.

## Payload used

The value inserted into the `email` field was:

```html
<iframe src="javascript:alert(`xss`)">
```

### What the payload means

| Component | Meaning |
|---|---|
| `<iframe>` | An HTML element used to embed another document or browsing context |
| `src` | The attribute specifying what the iframe loads |
| `javascript:` | A URL scheme containing JavaScript rather than a normal web address |
| `alert(...)` | A JavaScript function that displays a browser dialog |
| `` `xss` `` | A JavaScript string containing the text shown in the dialog |

In this vulnerable rendering context, the payload caused JavaScript to execute.

The test did not attempt to steal information, send data to an external service, or perform actions on another system. It used a visible alert as a limited proof of execution.

## Test procedure

### 1. Create a direct API request in OWASP ZAP

I used the Requester tab in OWASP ZAP to prepare a request to the user-creation endpoint:

```http
POST /api/Users HTTP/1.1
Host: localhost:3000
Content-Type: application/json
```

Request body:

```json
{
  "email": "<iframe src=\"javascript:alert(`xss`)\">",
  "password": "xss"
}
```

The backslashes before the double quotes are JSON escaping. They allow the HTML attribute quotes to appear inside a JSON string.

They are not a separate XSS technique.

### 2. Submit the payload directly to the backend

I sent the request without using the normal registration form.

This bypassed the client-side submission path and tested whether the backend would independently reject the unexpected value.

### 3. Observe successful account creation

The server responded:

```http
HTTP/1.1 201 Created
Location: /api/Users/27
Content-Type: application/json; charset=utf-8
```

Relevant response fields:

```json
{
  "status": "success",
  "data": {
    "role": "customer",
    "id": 27,
    "email": "<iframe src=\"javascript:alert(`xss`)\">",
    "updatedAt": "2026-10-12T04:50:52.569Z",
    "createdAt": "2026-10-12T04:50:52.569Z"
  }
}
```

This excerpt omits unrelated fields from the full response.

The response confirmed that:

- the request was accepted;
- a user record was created;
- the created record had ID `27`;
- the account role was `customer`;
- the submitted payload was returned in the `email` field.

An HTTP `201 Created` response alone does not prove XSS. It establishes successful submission and account creation. Browser execution is a separate observation.

### 4. View the administration page

I subsequently viewed:

```text
http://localhost:3000/#/administration
```

The browser displayed a dialog containing:

```text
xss
```

This was the proof that JavaScript executed in the browser.

### 5. Inspect the created user record

The administration interface also displayed the details of User #27.

The email field contained:

```html
<iframe src="javascript:alert(`xss`)">
```

The record's creation and update timestamps matched the API response.

This connected the submitted request to the stored user record visible in the application.

## Evidence and limitations

### Screenshot 1 — Direct API request and account creation

The first screenshot shows:

- OWASP ZAP Requester;
- `POST http://localhost:3000/api/Users`;
- the payload in the JSON `email` field;
- `HTTP/1.1 201 Created`;
- `Location: /api/Users/27`;
- the injected value returned in the response.

### Screenshot 2 — JavaScript execution

The second screenshot shows:

- the administration route;
- a browser alert associated with `localhost:3000`;
- the alert text `xss`.

This demonstrates execution, rather than merely storage or reflection of the payload.

### Screenshot 3 — Persisted user data

The third screenshot shows:

- the details dialog for User #27;
- the injected payload in the email field;
- creation and update timestamps matching the API response.

The payload is displayed as text in this particular details dialog. That does not contradict execution elsewhere on the page: different components can render the same value in different ways.

### What is confirmed

- The backend accepted the injected value in an account-creation request.
- The account was created as User #27.
- The administration interface displayed an `xss` alert.
- The payload remained visible in the created user's details.

### What is not established by these screenshots alone

- The exact source-code function or rendering sink responsible for execution.
- The precise implementation of the client-side protection.
- Whether a separate normal-form submission rejected the same payload.
- A challenge-completion notification or scoreboard entry.
- Account takeover, data theft, or any other impact beyond the demonstrated alert.

I report the challenge as completed based on my exercise outcome. The attached evidence documents the underlying submission, persistence, and execution.

## Why the attack worked

The observed sequence was:

```text
Direct API request
        ↓
Unexpected HTML/JavaScript accepted in the email field
        ↓
User record created
        ↓
Stored user data consumed by the administration page
        ↓
Unsafe rendering of the supplied value
        ↓
JavaScript execution
```

There were two important failures.

### 1. The backend accepted inappropriate input

The `email` field accepted HTML instead of an email address.

Client-side restrictions did not protect the API because the request reached the backend directly.

### 2. A display path treated untrusted data as executable content

Accepting the payload did not, by itself, cause the browser alert.

Execution required a later rendering path that interpreted the value as active content instead of displaying it safely as text.

The evidence supports unsafe rendering on the administration page. It does not establish the exact framework method or source-code implementation.

## Stored XSS versus reflected XSS

This exercise demonstrates a persisted payload.

The value was submitted during account creation and later appeared in the administration interface.

That differs from reflected XSS, where a payload is typically returned in an immediate response to a request without needing to be stored for later use.

The fact that the API echoed the value in JSON was not the execution point. The observed execution occurred in the browser while viewing the administration page.

## Potential security impact

In a real application, a similar stored XSS issue could affect users who view the contaminated record.

Depending on the application, browser protections, and victim's permissions, potential consequences could include:

- reading information available to the affected page;
- altering the page's appearance or content;
- issuing requests through the victim's browser;
- presenting deceptive prompts;
- interfering with administrative workflows.

These are possible consequences of the vulnerability class, not outcomes demonstrated in this exercise.

My test confirmed only execution of a harmless alert.

The user-creation response also showed a `customer` role. The exercise did not demonstrate that the submitted account gained administrator privileges.

## Remediation recommendations

### 1. Enforce validation on the server

Validate the email field at the backend, regardless of how the request is submitted.

Reject data that does not satisfy the application's email rules.

Client-side validation can improve usability, but it must not be relied upon as the sole security control.

### 2. Render account data as text

Email addresses do not require HTML rendering.

Use framework-default escaped text bindings or safe text APIs when displaying them.

Do not insert user-controlled account values into an HTML-parsing sink.

### 3. Apply context-appropriate output encoding

Output encoding must match the location where the value is inserted.

A value displayed in HTML text has different requirements from one inserted into an attribute, URL, or JavaScript context.

Encoding should ensure that user input remains data rather than becoming browser instructions.

### 4. Avoid unnecessary trust overrides

Do not mark user-controlled values as trusted HTML or bypass framework sanitization merely to simplify display.

Review components that display user profiles, account lists, administration tables, and details dialogs.

### 5. Sanitize only where HTML is genuinely required

If a feature intentionally supports user-provided HTML, use a maintained HTML sanitizer with a restrictive configuration.

For an email field, HTML support is unnecessary; plain-text rendering is preferable.

### 6. Use Content Security Policy as defense in depth

A carefully configured Content Security Policy can reduce the impact of some script-injection paths.

It supplements safe rendering and validation. It does not replace them.

### 7. Test the API and every relevant display path

Security tests should cover:

- direct API submissions that bypass the form;
- unexpected markup in plain-text fields;
- stored values rendered in lists and details dialogs;
- ordinary user views and administration views;
- regression checks confirming that payloads are rejected or displayed as inert text.

## Skills practiced

- Preparing and sending HTTP requests in OWASP ZAP Requester.
- Working with JSON request bodies.
- Understanding JSON escaping.
- Reading HTTP status codes and response headers.
- Connecting an API response to a persisted application record.
- Distinguishing successful submission from actual browser execution.
- Documenting observed evidence without claiming untested impact.

## Key takeaway

A browser-side security check can be skipped by sending a request directly to the backend.

Preventing stored XSS therefore requires more than validating the normal form: the server must enforce input rules, and every display component must treat untrusted values safely.

In this exercise, the API accepted executable markup as account data, and a later page rendered that data in a way that allowed JavaScript execution.
