# [Limit overrun race conditions] — [PortSwigger] — [Apprentice]
**Date:** 2026-08-06
 
---
 
## Vulnerability / core concept
Race Conditions

---


## Payload / key command
This application allows users to apply coupon during purchase. However, it lacks protections against race conditions. To exploit race conditions I used Burp's `Send group (parallel)` functionality in Repeater. First I intercepted the `Apply coupon` request and created 70 exactly same requests. After creating all these request I grouped them and using `Send group (parallel)` function sent them altogether to web application. After doing so, when we reload the page, we can notice that we applied this coupon for 70 times and now 1337.00$ has become much less than $50.00. Now we can purchase this jacket and solve this lab.


