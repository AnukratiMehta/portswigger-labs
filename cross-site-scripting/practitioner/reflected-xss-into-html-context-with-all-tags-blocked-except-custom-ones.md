# Lab: Reflected XSS into HTML context with all tags blocked except custom ones

**End Goal:** Perform a cross-site scripting attack that injects a custom tag and automatically alerts `document.cookie`.

**Analysis**

- Open Burp --> Proxy tab --> Open Browser --> Access lab
- Proxy --> HTTP history --> find the GET /?search=test request --> right-click it --> Send to Intruder
- Intruder tab --> Clear § --> Replace the search value with angle brackets --> select the spot between < and > and click Add §
- PortSwigger XSS cheat sheet --> Copy tags to clipboard --> Paste into the payload list --> Start Attack
- In the results window, look at the Status (or Length) column --> everything is blocked (403)

*Every standard tag is blocked. The lab name is the hint: all tags are banned except custom ones. A custom tag is just any made-up element name the browser doesn't recognise, like `<xss>`. The WAF has no list of those to block, so it sails through.*

- Search payload `<xss>` --> the response reflects it with 200, so custom tags are allowed

*Now I need the tag to run JavaScript on its own. A custom tag has no behaviour of its own, so I hang an event handler on it. `onfocus` fires when the element receives focus, and I can force focus without the victim clicking anything using two tricks together:*

- `tabindex=1` makes the custom element focusable (normally only links, inputs, etc. can be focused).
- A URL fragment `#x` that matches the element's `id=x` tells the browser to jump to that element on page load, which focuses it.

*So loading the page with `#x` auto-focuses my `<xss id=x>`, firing `onfocus` --> `alert(document.cookie)`, no interaction needed.*

- My search payload is `<xss id=x onfocus=alert(document.cookie) tabindex=1>` and the URL ends with `#x`
- To fire it against the victim with no clicks, I host a page on the exploit server that redirects the browser to that lab URL
- The payload is URL-encoded so it's valid inside the `location` value:

```
<   --> %3C
>   --> %3E
space --> + (or %20)
(   --> %28
)   --> %29
=   --> %3D
```

*The `#x` stays un-encoded at the very end so the browser treats it as the fragment and performs the focus jump.*

- Go to exploit server --> Body field --> paste the redirect script below --> Store
- View exploit --> the alert pops with the cookie --> Deliver exploit to victim

Decoded search payload: `<xss id=x onfocus=alert(document.cookie) tabindex=1>` with `#x` appended to the URL

**Answer:**

<script>
location = 'https://0a7a000303cff26e80a9ad1100eb001e.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29+tabindex%3D1%3E#x';
</script>




