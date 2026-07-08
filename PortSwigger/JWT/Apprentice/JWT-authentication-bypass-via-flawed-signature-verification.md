# [JWT authentication bypass via flawed signature verification] — [PortSwigger] — [Apprentice]
**Date:** 2026-07-08
 
---
 
## Vulnerability / core concept
JWT

---


## Payload / key command
This web application uses JWT tokens to handle users' session, authentication, authorization, etc. However, there is a problem with it: server does not properly reject the insecure tokens without signature. When we decode the JWT token we see the following:
```bash
{
  "iss": "portswigger",
  "exp": 1783540014,
  "sub": "wiener"
}
```
By using JWT encoder, we can change the sub to `administator` and set `alg` to `none` to sign it. Using this JWT token we can visit `/admin` page and delete user `carlos` to solve this lab.


