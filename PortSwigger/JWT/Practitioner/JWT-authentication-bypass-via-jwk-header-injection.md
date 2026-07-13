# [JWT authentication bypass via jwk header injection] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-13
 
---
 
## Vulnerability / core concept
JWT

---


## Payload / key command
This web application uses JWT with `kid`.To expose it we need to create a new key pair of our own, change `wiener` to `administrator`, sign it, and using that new generated JWT token access `/admin` endpoint to delete user carlos and solve this lab.
To do so:
1) We need to log in to intercept a request with JWT token.
2) Afterwards, we need to send this request to Repeater. Using `JWT Editor`, we should create a new RSA256 key and sign our request with JSON Web Token tab in Repeater.
3) After selecting the sign it, we choose attack-> Embedded JWK.
4) Now we can access the administrator page and delete user carlos.

