# Host header authentication bypass — (PortSwigger — Apprentice)
**Date:** 2026-08-09
 
---
 
## Vulnerability / core concept
Admin Panel by Host Header

---


## Payload / key command
This application has `/admin` endpoint that is available only to local users. However, authentication mechanism of this application is not secure. 

At first I tried to send ambigious requests, guess headers but nothing worked. Then, after reading the error message carefully, I tried to use `localhost` and `127.0.0.1` as `Host` header and `localhost` worked!

By sending requests like these we can access admin functionality and delete user `carlos` to solve this lab:
```bash
GET /admin/delete?username=carlos HTTP/2
Host: localhost:80
...
```


