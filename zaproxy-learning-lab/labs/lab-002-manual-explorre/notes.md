# Lab 002 - Manual Explore

## Objective

Learn how to perform an Manual Explore using ZAProxy against
OWASP Juice Shop.

## Environment

Target:
http://localhost:3000

Scanner:
ZAP

Date:
2026-09-26

## Steps Performed

1. Started Juice Shop
2. Started ZAP
3. Opened Manual Explore
4. Entered target URL
5. Enabled Manual Explore
6. Logged in to OWASP juice-shop as a user
7. Started exploring the site to build site tree within ZAP
8. Choose a site from the API site tree and attack with ZAP 

## ZAP Components Used

- Manual Explore
- Attack
- Alerts

## Observations

- ZAP built site tree following my manual exploring.
- Attack was later triggered by me manually against specific sites.
- Multiple medium alerts appeared.

## Challenges

- Understanding what actions I need to perform: enabling manual explore, log in as user, explore the site what resulted in site tree cration, attach chosen sites

## Lessons Learned


## Next Steps

- Try SQL injection following discovered alerts
- Compare results.

