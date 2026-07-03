# [Web cache poisoning via a fat GET request] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-04
 
---
 
## Vulnerability / core concept
Web Cache Poisoning

---


## Payload / key command
This web application executes `/js/geolocate.js?callback=setCountryCookie` when user loads home page to set the cookie. The problem with this application is that it allows GET request have body that leads to web cache poisoning via fat GET request. 
```bash
GET /js/geolocate.js?callback=setCountryCookie HTTP/2
...

callback=alert(1)
```

When we are using this request cache takes `setCountryCookie` as a key, but uses `alert(1)` as a actual function. This leads to arbitrary js execution in victims' browser.


