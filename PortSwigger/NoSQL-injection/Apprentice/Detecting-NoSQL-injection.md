# Detecting NoSQL injection — (PortSwigger — Apprentice)
**Date:** 2026-08-16
 
---
 
## Vulnerability / core concept
NoSQL Injection

---


## Payload / key command
This web application uses MongoDB database and is vulnerable to NoSQL injection.

To exploit it, we should visit `/filter?category=` endpoint and start testing it by appending different characters like `'`, `"`, `{}`, etc. During this testing we can notice that application breaks when we are appending `'`. This suggests us that user input is not validated and application processes our `'`. Now, to retrieve all items, even unpublished ones, `/filter?category=` should return `true` which means all products. We can achieve this sending request to `/filter?category?=Pets'||'1'='1`. `Pets'||'1'='1` gets closed by trailing `'` and becomes a valid boolean expression, where `Pets` is `true` because it is not an empty string and 1 is always equal to one. As this returns true, we get all products and solve this lab.
