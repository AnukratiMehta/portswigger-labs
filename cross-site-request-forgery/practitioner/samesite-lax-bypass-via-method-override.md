# Lab: SameSite Lax bypass via method override

**End Goal:** Host an HTML page on the exploit server that performs a CSRF attack to change the victim's email address.

**Login:** `wiener:peter`

**Analysis**

- Access lab --> log in as wiener --> go to **My account**
- Open DevTools --> **Network** tab --> submit a test email
- Click the `change-email` entry --> Headers show it's a `POST`, Payload shows just an `email` param --> there's no CSRF token at all
- DevTools --> **Application** --> **Cookies** --> the `session` cookie has no `SameSite` attribute set

*With no CSRF token, the only thing protecting this is the cookie's SameSite policy. No SameSite attribute means the browser defaults to Lax, and Lax won't send the session cookie on a cross-site POST, so a normal auto-submitting form won't be authenticated. But Lax does still send the cookie on a top-level GET navigation. So if I can turn this state-changing POST into a GET, the cookie will ride along.*

*The lab name points at method override. Many frameworks let you override the HTTP method with a `_method` parameter, so a GET carrying `_method=POST` gets handled as a POST.*

- Test it in the address bar (a top-level GET navigation) while logged in:

```
https://0aeb006904614bdd82633a9500e100f1.web-security-academy.net/my-account/change-email?email=test2@test.com&_method=POST
```

- Load it --> My account now shows `test2@test.com` --> the override works and the GET carries my cookie

*So my exploit just needs to make the victim's browser do a top-level GET navigation to that URL. An `<img>` or `fetch` would be a cross-site subresource request and Lax would strip the cookie, so I navigate the whole page with `document.location` instead.*

- Click **Go to exploit server** --> paste the script into the Body --> Store
- **Deliver exploit to victim** --> their browser navigates to the override URL, Lax sends the session cookie on the top-level GET, the server treats it as the POST change-email --> email changes

**Answer:**

<script>
  document.location = "https://0aeb006904614bdd82633a9500e100f1.web-security-academy.net/my-account/change-email?email=hacker@evil-user.net&_method=POST";
</script>
