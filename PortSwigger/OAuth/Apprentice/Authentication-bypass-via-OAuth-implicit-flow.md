# [Authentication bypass via OAuth implicit flow] — [PortSwigger] — [Apprentice]
**Date:** 2026-07-17
 
---
 
## Vulnerability / core concept
OAuth

---


## Payload / key command
When we are completing the OAuth authentication, we can observe a POST request that sends the following data to `/authenticate` endpoint on client application:
```js
{"email":"carlos@carlos-montoya.net","username":"carlos","token":"OvbIL9r2GTvlJL9L76lR8356pCER_lDOJu-tf41f9Dq"}
```
When we are changing the `email` and `username` to `carlos@carlos-montoya.net`, `carlos` correspondingly, we can observe that we are not receiving any error. In fact, we are able to login as `carlos` and solve this lab.


