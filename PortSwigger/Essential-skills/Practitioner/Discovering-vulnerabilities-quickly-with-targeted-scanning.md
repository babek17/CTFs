# Discovering vulnerabilities quickly with targeted scanning — (PortSwigger — Practitioner)
**Date:** 2026-08-25
 
---
 
## Vulnerability / core concept
Using Burp Scanner to Discover XXE

---


## Payload / key command
This web application has very few features: home page, product page, and stock check. The most interesting one from these features is of course `Check stock` feature. To spend less time on testing this specific endpoint, I used Burp Suite's `Active scan` feature that will actively scan most popular vulnerabilites and will report them. Running this scan reported several critical and medium findings: critical `External service interaction`, critical `Out-of-band resource load`, and medium `XML injection` vulnerabilities.

After seeing this result the very first thing was using XXE payload that contains `XIInclude` and retrieve `/etc/passwd`. This will return the contents of the file with error and solve this lab.

