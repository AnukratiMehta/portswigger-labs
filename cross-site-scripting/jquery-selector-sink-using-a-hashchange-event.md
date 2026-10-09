# Lab: DOM XSS in jQuery selector sink using a hashchange event

**End Goal:** Deliver an exploit to the victim that calls the ```print()``` function in their browser.

**Analysis**

*The blog page scrolls to a post based on the URL hash. The JS reads `location.hash` and drops it into a jQuery `$()` selector.*

```
$('section.blog-list h2:contains(' + decodeURIComponent(window.location.hash.slice(1)) + ')')
```

*If the string handed to `$()` has a tag in it, jQuery builds that tag instead of searching. So I can inject through the hash. `<script>` won't run this way, so use `onerror` like the innerHTML lab.*

```
<img src=x onerror=print(1)>
```

*But the handler only runs on the `hashchange` event, which needs the hash to actually change. Loading the page with the hash already set does nothing. Tried adding `#post` to the URL directly, no reaction.*

*Goal says victim's browser, and there's a "Go to exploit server" button, so I need to deliver a page. Use an iframe pointing at the lab, load it with an empty `#` first, then change the hash with `onload` so `hashchange` fires.*

- Go to exploit server --> paste the answer into Body --> Store
- View exploit to test on myself --> print dialog pops
- Deliver exploit to victim --> solved

*Use my lab's blog URL (the web-security-academy.net one, not the exploit-server one). Outer attribute in double quotes, no double quotes inside `onload`, or it breaks.*

**Answer:**

<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print(1)>'"></iframe>




