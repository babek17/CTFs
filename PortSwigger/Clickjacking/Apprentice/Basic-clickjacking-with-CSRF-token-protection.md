# [Basic clickjacking with CSRF token protection] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-05
 
---
 
## Vulnerability / core concept
Clickjacking

---


## Payload / key command
This web application has a feature that deletes user account on a single click. By using a decoy website we can clickjack the victim into deleting their account without their intent. For that purpose we can use Clickbandit in Burp Suite to create the decoy website and deliver it to the victim.


