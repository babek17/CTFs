# [JWT authentication bypass via jku header injection] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-15
 
---
 
## Vulnerability / core concept
JWT

---


## Payload / key command
This web application does not use strict JWKs allowlisting. It fetchs from any `jku` URL, which checks the JWT token only cryptographically: if it is valid, then server trusts it. Therefore, we can exploit this by using the following attack.
1) In exploit server, we add the following `JSON` to the body:
```js
{
    "keys": [
    ]
}
```
2) Using Burp Suite JWT Editor, we need to generate new RSA key and parse the output into that body in our exploit server and store it:
```js
{
    "keys": [{
    "kty": "RSA",
    "e": "AQAB",
    "kid": "eca735b5-5435-4167-b48d-a17d1c2b6bc9",
    "n": "sB5QLS6Ik0UCDX2h5xmU8pvu9syAcH4rQVcxLPwteqeB8TRhR3Q_k49cDnDnP61BJt-m1576_-MiwLEoyRYE-eOgAZ-QVbNjhwFFgkZU8WVLI05ZFxS6fHUCl_MAGzxFnGJjz-Rl8uDi__6QuskCa9b4tG5ZKSVNflhge_UD7v1AIw603otUaZVagMB3qjHVG3Dug9Tle5XyiDbZ2GMGyPiidAldMFILA-NskyEsuS7mrMePQWQGLBhqJw89MUMfe_U2KWK4L3xfxv4RiH-Ev2h3TYcmG6LfEZ5MfF3vQcGUj-mEoDcDym177fyC2OLVn4znYCn91nqPtthsAZlPJw"
}
    ]
}
```
3) Now, in Repeater, we modify the JWT token using JSON Web Token tab. We change the `kid` to our generated `kid`, and add new `jku` header with url set to our exploit server.
4) Then, changing `sub` to `administator` and signing it will allow us to access admin panel and delete user `carlos`.
