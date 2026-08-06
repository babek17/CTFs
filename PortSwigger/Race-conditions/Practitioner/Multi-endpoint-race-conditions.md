# [Multi-endpoint race conditions] — [PortSwigger] — [Practitioner]
**Date:** 2026-08-06
 
---
 
## Vulnerability / core concept
Race Conditions

---


## Payload / key command
To exploit multi-endpoint race condition in this web-site we should first inspect the web application and identify its valuable endpoints that we can start test for race conditions.

After logging in, we can start ordering items from the home page. One of them is gift card that we can use for our maliciuos intent. During inspection, we can notice `/cart/checkout` endpoint that validates our purchase. We can chain this with `/cart` POST request to execute race conditions attack. The idea is simple: our intention is to sneak more expensive item to the confirmation after purchasing validation is done. This way we will be able to purchase an item with a much higher price.

To do so, we need to send both these requests to Repeater and use `Send group (parallel)` option to send both requests at the same time. It might not work on first attempt, therefore, we need to do this several times.


