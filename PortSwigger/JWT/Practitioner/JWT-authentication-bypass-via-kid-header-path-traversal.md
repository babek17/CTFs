# [JWT authentication bypass via kid header path traversal] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-15
 
---
 
## Vulnerability / core concept
JWT

---


## Payload / key command
This web application uses `HS256` for JWT signing and verification. We are given that `kid` is vulnerable to path traversal. Combining this with symmetric algorithms used to sign JWT, we can exploit the following attack:
1) We login to our account and intercept the request with JWT token and send it to Repeater.
2) In Repeater, using JSON Web Token tab, we edit the `kid` to path traversal to some static file. The best choice is `/dev/null` because it exists on all Linux systems.
3) Now, we need to create a new symmetric key using JWT Editor extension in Burp Suite. Instead of random secret, we choose specify secret and leave it empty to match `/dev/null`.
4) In JSON Web Token tab, we change `sub` to `administrator` and using newly generated symmetric key, we sign it and send the request. Now we can access admin panel and delete user `carlos` to solve the lab.


