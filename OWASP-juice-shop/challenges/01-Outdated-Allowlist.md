# Challenge: Outdated Allowlist

## Objective

Access via app an redirect url (crypto related) that is not available via UI but was left by developers in the code. 

## Solution

Using Dev Tools search thru the app code to find "redirect", "crypto" etc key words to find specific url. Access url from the app level e.g. http://your-juice-shop-host/redirect?to=<found_crypto_url>

## Relevant Concepts

(https://cheatsheetseries.owasp.org/cheatsheets/Unvalidated_Redirects_and_Forwards_Cheat_Sheet.html)

## Lessons Learned
Searching thru the entire app code ctr+shift+f

# Coding Challenge: Related to Outdated Allowlist

## Objective
Fix the code with crypto urls kept in Redirect Allow list set

## Solution
Delete the crypto urls from the Redirect Allow list set

## Lessons Learned
<img width="877" height="502" alt="image" src="https://github.com/user-attachments/assets/0793524f-7cbc-4ab1-b303-d344c5fb412e" />


