# Challenge: Repetitive Registration

## Objective

Follow the DRY (DO not Repeat Yourself) principle while registering a user.

## Solution

I used Tamper Dev, intercepted request, modified it by changing repeat password input to different than the Password field input.

## Relevant Concepts

https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html

## Lessons Learned

Here are the most common risks and harms resulting from improper input validation:

Injection Attacks (e.g., SQLi, Command Injection): Attackers insert malicious code into input fields, allowing them to read, modify, or delete database contents or execute arbitrary commands on the server.

Cross-Site Scripting (XSS): Unsanitized text input stored and displayed to other users allows attackers to execute malicious scripts in users' browsers, stealing sessions, cookies, or credentials.

Unauthorized Access & Privilege Escalation: Bypassing weak input checks on user roles, IDs, or permissions can allow unauthorized users to access, modify, or delete administrative or private data.

Denial of Service (DoS): Processing unvalidated, excessively large, or complex inputs (e.g., nested JSON/XML or deeply recursive patterns) can crash the application or exhaust system resources.

Data Corruption & Logic Flaws: Malformed or out-of-range input can disrupt business logic, leading to incorrect calculations, corrupted database entries, or unexpected system crashes.

# Coding Challenge: Related to ...

## Objective

NO CODING CHALLANGE

## Solution



## Lessons Learned


trigger anti-money laundering (AML) or payment processor violations (e.g., Stripe or PayPal restrictions).
