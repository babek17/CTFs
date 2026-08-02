# [Client-side prototype pollution in third-party libraries] — [PortSwigger] — [Practitioner]
**Date:** 2026-08-02
 
---
 
## Vulnerability / core concept
DOM XSS via Prototype Pollution

---


## Payload / key command
This web application uses third-party library which is vulnerable to prototype pollution. I solved this lab using DOM Intruder as javascript code was minified which made reading it much harder and chances of missing sink a lot.

After loading home web page, we can use DOM Invader to find a sink for our prototype pollution. Then, pressing `Scan for gadgets` we find the gadget that we can use to exploit prototype pollution. This is the payload that we need in the URI:
```js
/#__proto__[hitCallback]=alert%28document.cookie%29
```
To deliver this to victim, we need to preserve fragment. We can do this by using `location` which triggers full top-level navigation, which fragments survive.

In our exploit server we write the following script in the body:
```js
<script>
location = "https://0a5200f704c7b811815e57ec008c0062.web-security-academy.net/#__proto__[hitCallback]=alert%28document.cookie%29"
</script>
```
After delivering this link to victim we solve the lab.
