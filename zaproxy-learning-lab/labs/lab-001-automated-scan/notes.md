# Lab 001 - Automated Scan

## Objective

Learn how to perform an automated scan using ZAP against
OWASP Juice Shop.

## Environment

Target:
http://localhost:3000

Scanner:
ZAP

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
- Alerts

## Observations

- ZAP spider crawled the application.
- Multiple medium, low and informational severity alerts appeared.

## Challenges

- Learning how alert risk levels are assigned.
- getting familiar with alerts content and how they are linked to CWE and WASC items
- how to track down specific weakness via its ID on WASC page: http://projects.webappsec.org/w/page/13246974/Threat%20Classification%20Reference%20Grid 

## Lessons Learned

- Spider discovers content.
- Alerts should always be validated manually.
- Specific alert provides type of weekens

## Next Steps

- Learn traditional Spider.
- Learn AJAX Spider.
- Compare results.
