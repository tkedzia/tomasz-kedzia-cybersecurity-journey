# Lab 001 - Automated Scan

## Objective

Learn how to perform an automated scan using OWASP ZAP against
OWASP Juice Shop.

## Environment

Target:
http://localhost:3000

Scanner:
OWASP ZAP

Date:
2026-09-19

## Steps Performed

1. Started Juice Shop
2. Started ZAP
3. Opened Automated Scan
4. Entered target URL
5. Enabled Attack Mode
6. Started scan

## ZAP Components Used

- Spider
- Passive Scan
- Active Scan
- Alerts

## Observations

- ZAP first crawled the application.
- Active scan started automatically.
- Multiple medium and low severity alerts appeared.

## Challenges

- Understanding difference between Spider and Active Scan.
- Learning how alert risk levels are assigned.

## Lessons Learned

- Spider discovers content.
- Passive scan analyzes traffic.
- Active scan sends attack payloads.
- Alerts should always be validated manually.

## Next Steps

- Learn traditional Spider.
- Learn AJAX Spider.
- Compare results.
