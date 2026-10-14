# Lab: Reflected XSS with some SVG markup allowed

**End Goal:** Perform a cross-site scripting attack that calls the `alert()` function.

**Analysis**

- Open Burp --> Proxy tab --> Open Browser --> Access lab
- Proxy --> HTTP history --> find the GET /?search=test request --> right-click it --> Send to Intruder
- Intruder tab --> Clear § --> Replace the search value with angle brackets --> select the spot between < and > and click Add §
- PortSwigger XSS cheat sheet --> Copy tags to clipboard --> Paste into the payload list --> Start Attack
- In the results window, look at the Status (or Length) column --> most tags blocked, but some SVG tags get through with 200: `svg`, `animatetransform`, `title`, `image`

*So SVG tags survive the filter. A tag on its own doesn't run JS though, so I need an event handler that is allowed on one of these elements. Fuzz events next.*

- `<svg><animatetransform §§=1>` --> PortSwigger XSS cheat sheet --> Copy events to clipboard --> Paste into the payload list --> Start Attack
- In the results, `onbegin` comes back 200

*`onbegin` is an SMIL animation event: it fires automatically when the animation element begins, as soon as the page loads. No victim interaction needed. `<animatetransform>` has to live inside an `<svg>` to be valid, which is why I wrap it.*

**Answer:**

<svg><animatetransform onbegin=alert(1)>

