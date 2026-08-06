# [Single-endpoint race conditions] — [PortSwigger] — [Practitioner]
**Date:** 2026-08-06
 
---
 
## Vulnerability / core concept
Race Conditions

---


## Payload / key command
In this application we can see a sensitive endpoint `/my-account/change-email`. We send this request when we are trying to change our email address. This request sends a confirmation link to the new email address so we can confirm that submitted email address belongs to us. However, this application did not implement security guards against race conditions. 

To exploit this race condition we need to do the following:
1) First, we need to intercept `/my-account/change-email` and send two of those request to Repeater: one with email that belongs to us, the other with `carlos@ginandjuice.shop`
2) Now, we can use `Send group (parallel)` several times to find the race window and make victim to receive our email confirmation and us to receive victim's confirmation.

After doing these, we can see that we received the confirmation letter that was supposed to be sent to `carlos@ginanjuice.shop` but got sent to us. Now we can access `Admin panel` and remove user carlos to solve this lab.

