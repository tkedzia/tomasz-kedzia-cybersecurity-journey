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

Key Reasons to Use an Allow List to Block Crypto URLs

- Mitigation of Cryptojacking & Malicious Mining Scripts
Attackers often try to inject malicious JavaScript into web applications (via XSS or compromised third-party dependencies/libraries) to mine cryptocurrency using your visitors' or servers' CPU power. An allow list prevents the browser or server from fetching mining scripts or connecting to web-socket pools hosted on crypto-related domains.

- Protection Against Server-Side Request Forgery (SSRF)
If your application has features that fetch URL content on behalf of users (e.g., generating link previews, fetching webhooks, or downloading external files), attackers will try to manipulate those features. If you rely on a blocklist, attackers can use URL shorteners, open redirects, or new crypto domains to bypass your filter. An allow list guarantees your server only communicates with a pre-approved list of trusted domains (e.g., api.github.com, images.unsplash.com), automatically blocking all crypto platforms by default.

- Prevention of Data Exfiltration
If an attacker successfully compromises your site, they need a server to send stolen user data (cookies, tokens, payment details) to. Malware authors frequently use decentralized crypto infrastructure or domain-fronting services to hide their command-and-control (C2) servers. An allow list stops your application from making outbound requests to unauthorized external networks.

- Regulatory, Brand, & Financial Compliance
Crypto platforms, mixing services, and Web3 endpoints are frequently targeted by regulatory bodies, sanctioned, or associated with high-risk financial transactions. Enforcing a strict allow list prevents your infrastructure from inadvertently interacting with sanctioned addresses or hosting links that could trigger anti-money laundering (AML) or payment processor violations (e.g., Stripe or PayPal restrictions).

