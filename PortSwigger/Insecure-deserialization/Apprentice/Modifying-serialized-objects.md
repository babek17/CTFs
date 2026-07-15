# [Modifying serialized objects] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-15
 
---
 
## Vulnerability / core concept
Insecure Deserialization

---


## Payload / key command
This web application uses PHP serialized cookie to determine the user and its privilege. The cookie is URL encoded -> Base64 encoded string that represent object:
```bash
O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:0;}
```
This is what it looks like after we decode it. Now all we need to do is to change `admin` attribute from 0 to 1 and access admin panel. Afterwards, we can delete user `carlos` and solve this lab.


