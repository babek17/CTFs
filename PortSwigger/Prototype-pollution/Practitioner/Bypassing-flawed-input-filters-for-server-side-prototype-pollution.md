# [Bypassing flawed input filters for server-side prototype pollution] — [PortSwigger] — [Practitioner]
**Date:** 2026-08-05
 
---
 
## Vulnerability / core concept
Server-side Prototype Pollution

---


## Payload / key command
After logging in to our account, we can edit the details about our user. Intecepting this request allows us to modify JSON body and send it to the server.

When I tried to use `__proto__` in JSON body, application did not react to it. This might be input filters that does not allow attacker use `__proto__` to pollute. At this point I decided to use `constructor`. I used the following JSON body:
```js
{
   "address_line_1":"Wiener HQ",
   "address_line_2":"One Wiener Way",
   "city":"Wienerville",
   "postcode":"BU1 1RP",
   "country":"UK",
   "sessionId":"0WHkI64pM0oxRScnOZZ8gg5aEfjNyVPE",
   "constructor":{
      "prototype":{
         "isAdmin":true
      }
   }
}
```
This payload worked: application filtered the `__proto__` but not `constructor`. Now we can access admin panel and delete user `carlos` to solve this lab.

