# [DOM XSS via client-side prototype pollution] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-30
 
---
 
## Vulnerability / core concept
DOM XSS via Prototype Pollution

---


## Payload / key command
I solved this lab using DOM Invader.

When we load the home page of this web application we can notice the `constructor` and `prototype` words in the URI. This showcases potential prototype pollution. Using built-in Burp browser, we can turn on DOM Invader via Main settings and Prototype Pollution via Attack types. After reloading, our DOM Invader becomes active. Now, we need to open developer tools and see if DOM Invader caught anything. As a result, we can notice that DOM Invader caught prototype pollution using `__proto__[property]=value` in search. After pressing `Scan for gadgets` and `Exploit`, we exploit this lab. DOM Invader by default uses `alert(1)` as PoC.


