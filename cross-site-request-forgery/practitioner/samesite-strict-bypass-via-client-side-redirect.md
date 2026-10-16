# Lab: SameSite Strict bypass via client-side redirect

**End Goal:** Host an HTML page on the exploit server that performs a CSRF attack to change the victim's email address.

**Login:** `wiener:peter`

**Analysis**

- Access lab --> log in as wiener --> go to **My account**
- DevTools --> **Network** --> submit a test email --> the `change-email` request is a `POST` with just an `email` param and no CSRF token
- DevTools --> **Application** --> **Cookies** --> the `session` cookie is `SameSite=Strict`

*Strict is tougher than Lax: it won't send the cookie on any cross-site request, including a top-level navigation. So I can't drive the victim straight to the change-email URL from my exploit because the cookie would be stripped. The navigation has to originate from the lab's own site so it counts as same-site. I need an on-site redirect gadget I can point wherever I want.*

- Post a comment on any blog post --> I land on `/post/comment/confirmation?postId=X`, and a script (`/resources/js/commentConfirmationRedirect.js`) reads `postId` and redirects me to `/post/<postId>`

*It builds the redirect path by sticking `postId` straight onto `/post/`, with no validation. So I can use path traversal in `postId` to aim the redirect at any endpoint on the site. Because that redirect is run by the lab's own page, the follow-up request is same-site and the Strict cookie is sent.*

- Confirm the traversal: visit `/https://0a22004b036d3c2c80813a3b000a00d8.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account` --> the browser normalises the path and lands on My account --> traversal works
- Confirm change-email accepts GET: in the address bar visit

```
https://0a22004b036d3c2c80813a3b000a00d8.web-security-academy.net/my-account/change-email?email=test2@test.com&submit=1
```

- My account now shows `test2@test.com` --> the endpoint doesn't check the method, so a GET with `email` + `submit=1` changes it

*Now I combine the two: point the confirmation-page redirect at the change-email GET. The whole change-email query has to live inside `postId`, so I encode its `&` as `%26` (otherwise it would split off as a parameter of the confirmation URL) and `@` as `%40`.*

- Click **Go to exploit server** --> paste the script into the Body --> Store
- **Deliver exploit to victim** --> their browser loads the confirmation page, its own JS redirects (same-site) to the change-email GET, the Strict cookie is sent --> email changes

**Answer:**

<script>
document.location = "https://0a22004b036d3c2c80813a3b000a00d8.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=hacker@evil-user.net%26submit=1";
</script>
