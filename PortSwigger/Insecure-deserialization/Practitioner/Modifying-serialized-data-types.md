# [Modifying serialized data types] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-15
 
---
 
## Vulnerability / core concept
Insecure Deserialization

---


## Payload / key command
This web application uses the following PHP deserialization in Cookie to determine the session for user:
```bash
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"m5qt0hdijpz2ndgg341c9xw4nzahct33";}
```
As we learnt, in PHP <8 versions, loose comparison returns true when comparing int 5 with string "5". It automatically converts string to int and then compares it with other integers value. Moreover, when comparing int 5 and string "5 hello world", old versions of PHP still returns true because it ignores the string and only compares the integer part. This is why we can also get true if we compare 0 with some arbitrary string that does not start with a number. 

This functionality helps us to bypass authentication in this web application. We can modify the token above to:
```bash
O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}
```
In this token, we are declaring ourselves `administrator`. To bypass `access_token` we use the specification of PHP mentioned above: we compare the `access_token` of administrator (which is string) to 0. Unless `access_token` of administrator does not start with a number, we are able to bypass authentication mechanism and delete user `carlos` to exploit this lab.
