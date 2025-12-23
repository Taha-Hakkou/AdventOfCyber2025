## day 11

* XSS is a web application vulnerability that lets attackers inject malicious code (usually javascript) into input fields that reflects content viewed by other users. It can lead to stealing credentials, deface pages or impersonate users.

* types of xss (exist many other types..)

- reflected:
you see reflected variants when injection is immediately projected in a response.
example:
> https://trygiftme.thm/search?term=<script>alert( atob("VEhNe0V2aWxfQnVubnl9") )</script>
You could act, view information, or modify information that the victim or any user could do, view, or access.

- stored:
occurs when malicious script is saved on the server and then loaded for every user who views the affected page.
example: (inject js in a post comment)
```sh
comment=<script>alert(atob("VEhNe0V2aWxfU3RvcmVkX0VnZ30="))</script> + "This gift set my carpet on fire but my kid loved it!"
```
This lets the attacker run code as if they were the victim in order to perform malicious actions such as: Steal session cookies, Trigger fake login popups, or Deface the page.


- Reflected XSS targets individual victims,  while Stored XSS becomes a "set-and-forget" attack, anyone who loads the page runs the attacker’s script.

* protecting against XSS:
- Disable dangerous rendering raths: Instead of using the **innerHTML** property, which lets you inject any content directly into HTML, use the **textContent** property instead, it treats input as text and parses it for HTML.
- Make cookies inaccessible to JS: Set session cookies with the **HttpOnly**, **Secure**, and **SameSite** attributes to reduce the impact of XSS attacks.
- Sanitise input/output and encode: applications may need to accept limited HTML input—for example, to allow users to include safe links or basic formatting. Sanitising and encoding removes or escapes any elements that could be interpreted as executable code, such as scripts, event handlers, or JavaScript URLs while preserving safe formatting.

* exploiting reflected/stored XSS:
1. use test payloads to check if injected code runs. example:
```js
<script>alert('Reflected/Stored Meow Meow')</script>
```

2. use advanced payloads (with cheat sheet help)
> https://portswigger.net/web-security/cross-site-scripting/cheat-sheet