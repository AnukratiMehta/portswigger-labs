# Lab: CSRF where token validation depends on token being present

**End Goal:** Host an HTML page on the exploit server that uses a CSRF attack to change the viewer's email address.

**Login:** `wiener:peter`

**Analysis**

- Access lab --> log in as wiener --> go to **My account**
- Open DevTools --> leave **Network** tab open so it records requests
- Type `test@test.com` in the Update email box --> Submit
- A `change-email` entry appears in the Network tab --> click it
- **Headers** tab --> under "General", Request Method is `POST`
- **Payload** tab --> Form Data lists the two body fields, `email` and a `csrf` token

*So the POST sends a csrf token. The lab name is the hint: validation depends on the token "being present". That suggests the server only checks the token when the `csrf` field is actually there — if I leave it out entirely, there's nothing to validate and the request goes through.*

- Build a page with a hidden form that POSTs just the `email` param and auto-submits:

- Click **Go to exploit server** --> paste the HTML into the Body --> Store
- Click **View exploit** to test on my own account first --> My account email changes --> confirms the request succeeds with no csrf token
- **Deliver exploit to victim** --> the victim's browser POSTs with their session cookie and no token --> email changes --> lab solved

**Answer:**

<form action="https://0af600b0031d6e9b80bd26ed00e000ae.web-security-academy.net/my-account/change-email" method="POST">
  <input type="hidden" name="email" value="hacker@hack.net">
</form>
<script>
  document.forms[0].submit();
</script>

