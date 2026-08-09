# Routing-based SSRF — (PortSwigger — Practitioner)
**Date:** 2026-08-09
 
---
 
## Vulnerability / core concept
SSRF via HTTP Host Header Attack

---


## Payload / key command
We are given that this web application has an internal `192.168.0.0/24` subnet. There is an admin panel in one of these ip addresses that we need to find.

To do so, we start manipulating `Host` header. I tried to use ambigious requests but they did not work. Then I tried to change `Host` header value to `192.168.0.1` and send request. Response was `504 Gateway Timeout`. At first this confused me and made me try other things. Then I realized that if public and internal servers are virtual hosts and share same ip address, that means if I am getting Gateway Timeout, that might simply mean it doesn't exists. Therefore, I send that request to Intruder and brute-forced through `192.168.0.1` to `192.168.0.255`. This brute-force returned me one `302` status response, which was admin panel!

Now, after finding the right ip address we can send the following request to delete user `carlos` and solve this lab:
```bash
POST /admin/delete HTTP/2
Host: 192.168.0.123
...

username=carlos&csrf=oIYxyOlFqEzbA1JcU3E98Jb8TPxkPK6R
```


