# Host validation bypass via connection state attack — (PortSwigger — Practitioner)
**Date:** 2026-08-10
 
---
 
## Vulnerability / core concept
Host Validation Bypass

---


## Payload / key command
This server keeps connection alive for 10 seconds to ease the load on server. What it does is simple - validate the first request and if it is validated, pass other requests without validation for 10 second (while connection is alive).

To exploit this, we can use feature of Burp Suite -> `Send group (single connection)`. We need to have two request. First one is valid, boring request that gets validated. Second request is malicious, its `Host` header is set t o internal admin panel `192.168.0.1/admin`. By pressing `Send group (single connection)` we can see that we got access to the `/admin` endpoint. Now all we need to do is to send the following request using same `Send group (single connection` and solve this lab:

First request:
```bash
GET / HTTP/1.1
Host: 0a03007a039e616a823bb64300ae00e5.h1-web-security-academy.net
...
```
Second request (malicious):
```bash
GET /admin/delete?username=carlos&csrf=JjAIyS2GjTU9SewKdWBSuakWT8jtHJY5 HTTP/1.1
Host: 192.168.0.1
...
```

