# [Parameter Cloaking] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-04
 
---
 
## Vulnerability / core concept
Web Cache Poisoning

---


## Payload / key command
This web application imports the script `/js/geolocate.js`, executing the callback function `setCountryCookie()` when visiting the home page. I sent this request to Repeater and run Param Finder. I found out that this app uses `utm_content` query parameter, which is also excluded from cache key. I tried to inject malicious payload to `utm_content` but web application was HTML encoding it so it was uneffective. Therefore I started trying parameter cloaking with various methods. This one worked:
```bash
GET /js/geolocate.js?callback=setCountryCookie&utm_content=123;callback=alert(1)

...
```
Here it is also important to use `alert(1)` instead of whole `<script>` payload because it is already being executed in javascript.


