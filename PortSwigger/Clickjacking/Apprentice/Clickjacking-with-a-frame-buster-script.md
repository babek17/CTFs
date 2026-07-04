# [Clickjacking with a frame buster script] — [PortSwigger] — [Apprentice]
**Date:** 2026-07-05
 
---
 
## Vulnerability / core concept
Clickjacking

---


## Payload / key command
This web application allows to prepopulate form by using GET URL path. Therefore, we need to remove `?id=wiener` and add `?email=victim@pwnedd`. Then, using Clickbandit in Burp Suite, we can generate the decoy HTML that will deceive victim into changing their mail without their knowledge. Moreover, we need to check sandbox in the Clickbandit and use only `allow-forms` so we can create proper decoy page for victim because frame buster can detect if the iframe is top window or not without `allow-forms`.

```bash
https://0a5000bc03d188bc8287a37300fe003d.web-security-academy.net/my-account?email=victim@pwnedd
```

This was the URL used with Clickbandit to create decoy page.


