# OWASP Juice Shop — Manipulate Basket

> Scope: Self-hosted/local OWASP Juice Shop training environment only  
> Category: Broken Access Control / HTTP Parameter Pollution  
> Tool: OWASP ZAP  
> Authentication: Valid session as `jim@juice-sh.op`  
> Challenge: Manipulate Basket  
> Objective: Add a product to another user’s shopping basket by exploiting inconsistent handling of repeated `BasketId` parameters.

## Executive summary

I completed the “Manipulate Basket” challenge in a local OWASP Juice Shop training environment.

The application allowed me to submit a request containing two different values for the same field: `BasketId`. One value belonged to my authenticated account and the other belonged to another user.

The server accepted the request because one part of the application recognized my own basket as valid. However, another part of the application processed the other basket identifier and added the product to the target user’s basket.

This is a form of **parameter tampering**. The user changed a value in a request that should have been controlled exclusively by the server. The challenge demonstrates why web applications must not trust client-provided identifiers when deciding which user-owned object to modify.

## Environment and authorization

This exercise was performed only against a deliberately vulnerable, locally hosted OWASP Juice Shop instance.

| Item | Value |
|---|---|
| Application | OWASP Juice Shop |
| Target | `http://localhost:3000` |
| Scope | Local/self-hosted training environment |
| Tool | OWASP ZAP Manual Request Editor |
| Authenticated account | `jim@juice-sh.op` |
| Authentication method | Valid `Authorization: Bearer <token>` header |
| My basket ID | `1` |
| Target basket ID | `2` |
| Product ID used in the completed test | `22` |
| Quantity | `1` |

No external system, real customer account, or production application was tested.

## What a shopping basket request normally does

When a customer adds an item to an online basket, the browser sends a request to the application.

A simplified version of such a request contains:

```json
{
  "ProductId": 22,
  "BasketId": "1",
  "quantity": 1
}
```

In plain language, it says:

- add product number `22`;
- to basket number `1`;
- in a quantity of `1`.

A secure application should not let the browser freely choose the basket. The application already knows who is logged in from the session or Bearer token, so it should determine the correct basket internally.

The vulnerable application accepted a basket identifier sent by the browser. That gave the user an opportunity to alter a value that should not have been trusted.

## Why this is a security problem for a non-technical reader

Think of an online supermarket checkout system.

A customer is allowed to place products into their own trolley. The website verifies that the customer is logged in, which is similar to showing a membership card at the entrance.

However, the request also includes a label saying which trolley should receive the product. Normally, the system should ignore that label and use the trolley assigned to the logged-in customer.

Instead, the vulnerable application trusted the label supplied by the browser.

The problem became more serious because the application received two trolley labels with different numbers. One part of the system checked the first or one of those labels and saw a valid trolley owned by the logged-in person. Another part used the other label when performing the actual update.

As a result:

1. the authorization check effectively said, “Yes, this user owns basket `1`”;
2. the product operation effectively said, “Add the item to basket `2`”;
3. the system did not verify that the final basket being modified belonged to the authenticated user.

The login itself was valid. The failure was in authorization: the application did not consistently verify whether the logged-in user was allowed to modify the basket that was ultimately changed.

## Initial test: direct manipulation was rejected

First, I attempted a direct modification by supplying only another user’s basket identifier:

```json
{
  "ProductId": 22,
  "BasketId": "2",
  "quantity": 1
}
```

This attempt was rejected with an error similar to:

```json
{
  "error": "Invalid BasketId"
}
```

This response shows that the application did have an authorization check. It recognized that basket `2` did not belong to my authenticated account.

However, the protection was incomplete because it did not safely handle repeated parameter names.

## Successful test: repeated `BasketId` values

I then used OWASP ZAP Manual Request Editor to send a request that included the `BasketId` field twice:

```json
{
  "ProductId": 22,
  "BasketId": "2",
  "quantity": 1,
  "BasketId": "1"
}
```

The request included:

- a valid Bearer token for my authenticated test account;
- one basket ID associated with my own session;
- another basket ID associated with the target basket;
- the same field name repeated twice.

The application returned `HTTP/1.1 200 OK` and confirmed that it had created a basket-item record.

```json
{
  "status": "success",
  "data": {
    "id": 13,
    "ProductId": 22,
    "BasketId": "1",
    "quantity": 1,
    "updatedAt": "2026-10-12T03:59:35.115Z",
    "createdAt": "2026-10-12T03:59:35.115Z"
  }
}
```

The Juice Shop interface then displayed a success message confirming that the Manipulate Basket challenge had been completed.

## Evidence

### 1. Crafted request in OWASP ZAP

The screenshot below shows the request created in OWASP ZAP Manual Request Editor.

Important elements visible in the request:

- `POST /api/BasketItems`;
- a valid `Authorization: Bearer` header;
- `Content-Type: application/json`;
- repeated `BasketId` fields in the JSON body;
- product ID `22`;
- quantity `1`.

> Sensitive session material is visible in the original capture. Do not publish a live Bearer token in a public repository. Replace it with `<redacted-token>` before committing screenshots, or crop/redact the `Authorization` header.

![Crafted POST request in OWASP ZAP](image.jpg)

### 2. HTTP response

The server returned `HTTP/1.1 200 OK`.

The response indicates that a basket-item record was created successfully. The fact that the request was accepted despite conflicting basket identifiers is the central issue demonstrated by this challenge.

![HTTP 200 response in OWASP ZAP](image-2.jpg)

### 3. Challenge completion confirmation

The Juice Shop interface confirmed successful completion of the challenge:

> You successfully solved a challenge: Manipulate Basket  
> Put an additional product into another user's shopping basket.

![OWASP Juice Shop challenge success notification](image-3.jpg)

## Technical explanation: HTTP Parameter Pollution

This issue is related to HTTP Parameter Pollution, often abbreviated as HPP.

Parameter pollution occurs when an HTTP request supplies the same parameter more than once. Different technologies can interpret repeated parameters differently:

- one component may use the first value;
- another component may use the last value;
- a framework may turn both values into a list;
- a validator may inspect one value while the database operation uses another;
- some parsers may silently overwrite an earlier value.

In this challenge, the application processed duplicate `BasketId` fields inconsistently.

The exact internal implementation is intentionally vulnerable as part of the Juice Shop training application. The important lesson is that validation and the business action must operate on the same, unambiguous value.

A secure application must not validate basket `1` and then perform an update against basket `2`.

## Authentication versus authorization

This scenario is a useful example of the difference between authentication and authorization.

### Authentication: “Who are you?”

Authentication confirmed that I had a valid session as `jim@juice-sh.op`.

The Bearer token proved that the request came from an authenticated user.

### Authorization: “Are you allowed to do this?”

Authorization should have confirmed that the basket being changed belonged to that authenticated user.

The application should have verified, immediately before changing the basket, that:

```text
requested basket belongs to authenticated user
```

Instead, inconsistent handling of duplicate values let the validation and the modification refer to different baskets.

A valid login does not give a user permission to alter every object in the application.

## Potential business impact

In a real e-commerce environment, a similar vulnerability could allow an attacker with an ordinary customer account to interfere with other customers’ baskets.

Possible consequences include:

- adding unwanted items to another customer’s basket;
- causing confusion during checkout;
- manipulating promotions, discounts, stock reservations, or basket totals;
- generating support requests and operational costs;
- testing whether related endpoints expose more serious authorization weaknesses;
- using the same flawed logic against addresses, orders, payment preferences, or account records.

The impact depends on what other application functions trust the manipulated object identifier. A basket modification may seem low severity on its own, but it can indicate a wider broken access-control pattern.

## Root cause

The root cause is not simply that the request contained two fields with the same name.

The underlying failure is that the application:

1. accepted a client-controlled basket identifier;
2. allowed duplicate security-sensitive fields;
3. did not normalize the request into one canonical value before validation;
4. did not ensure that the authorization check and the data-update operation used the same final basket identifier;
5. relied on client-side input for an ownership decision that should have been enforced server-side.

## Secure design

A robust implementation should derive the basket from the authenticated session instead of accepting it from the client.

Conceptually, the secure server-side behavior should resemble:

```text
authenticatedUser = getUserFromSessionOrToken()
basket = getActiveBasketForUser(authenticatedUser)
createBasketItem(basket.id, productId, quantity)
```

The browser should only need to provide product and quantity:

```json
{
  "ProductId": 22,
  "quantity": 1
}
```

The server should determine the basket internally.

## Remediation recommendations

### 1. Do not trust a client-supplied basket identifier

The safest approach is to remove `BasketId` from the public request entirely.

The server should obtain the active basket from the authenticated user’s session or token. This prevents the client from selecting another person’s basket in the first place.

### 2. Reject duplicate security-sensitive parameters

The application should reject requests containing duplicate fields such as:

- `BasketId`
- `userId`
- `accountId`
- `orderId`
- `role`
- `tenantId`

Duplicate keys should result in a clear `400 Bad Request` response rather than being silently accepted.

### 3. Use one canonical parsed value

The validation layer, authorization layer, and database-update logic must use the same normalized request object.

A request must not be validated with one interpretation of `BasketId` and executed with another.

### 4. Apply object-level authorization immediately before the action

Before creating or modifying an item, the application should verify:

```text
Does this authenticated user own, or have explicit permission to modify, this exact basket?
```

This validation should happen on the server for every request, not only in the user interface.

### 5. Add negative authorization tests

Automated tests should verify that an authenticated ordinary user cannot:

- add items to another user’s basket;
- modify another user’s basket item;
- remove another user’s basket item;
- submit duplicated `BasketId` values;
- use conflicting object identifiers in URL parameters, JSON bodies, headers, or form data.

### 6. Log suspicious malformed requests

Repeated security-relevant keys can indicate probing or attempted abuse. The application should log rejected duplicate-parameter requests and make them visible to security monitoring.

## Key takeaway

This challenge demonstrates that a web application must consistently control and authorize object identifiers.

The user was legitimately logged in, but that did not mean they should have been able to decide which basket was modified. The vulnerability existed because the application validated one basket identifier while processing another.

The secure approach is simple: determine the user’s basket on the server, reject ambiguous requests, and enforce authorization against the exact object that will be changed.
