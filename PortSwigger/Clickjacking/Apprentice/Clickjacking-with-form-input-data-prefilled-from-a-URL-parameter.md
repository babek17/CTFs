# [Clickjacking with form input data prefilled from a URL parameter] — [PortSwigger] — [Apprentice]
**Date:** 2025-07-05
 
---
 
## Vulnerability / core concept
Clickjacking

---


## Payload / key command
This web application allows repopulating form via GET URL before submission. Knowing this, we can populate the URL with value that we want, remove our own `id` from it, and using Clickbandit in Burp Suite craft a clickjacking HTML that we can send to the victim to change their email.

```bash
https://0a8900000380b53280a8b290008f0037.web-security-academy.net/my-account?email=babek@babakkk
```

This is the malicious link that we will send to them. Their `id` will populate automatically and their email will be changed.


