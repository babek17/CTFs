# SSRF via flawed request parsing — (PortSwigger — Practitioner)
**Date:** 2026-08-09
 
---
 
## Vulnerability / core concept
SSRF via HTTP Host Header Attack

---


## Payload / key command
Public and internal server of this application share the same ip address. That means we can alternate our request to make server-side request forgery to their internal web-app.

To do so, I tried several Host header attack techniques. Here what has worked:

We can pass the absolute URL to the path and see that doing this bypassed the validation of invalid Host header, which we were having before:
```bash
GET https://0aad00b70369fa688026e9e000b4008d.web-security-academy.net/ HTTP/2
Host: 192.168.0.x
...
```
Now, we are getting `504 Gateway Timeout` error in the response. It is time to use Intruder to brute-force from `192.168.0.1` to `192.168.0.255` and find the existing internal admin panel.

In my case it was `192.168.0.190`. To solve the lab we need to send the following request and delete user `carlos`:
```bash
GET https://0aad00b70369fa688026e9e000b4008d.web-security-academy.net/admin/delete?username=carlos&csrf=sGub5Aq2XQ5aPrQI47yee7BLUKAvdzSs HTTP/2
Host: 192.168.0.190
...
```

