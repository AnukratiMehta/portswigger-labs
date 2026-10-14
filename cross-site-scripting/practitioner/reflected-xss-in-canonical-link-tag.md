# Lab: Reflected XSS in canonical link tag

**End Goal:** Perform a cross-site scripting attack on the home page that injects an attribute that calls the `alert` function.

**Analysis**

- Open Burp --> Proxy tab --> Open Browser --> Access lab
- View the home page source

<link rel="canonical" href='https://0adf008f04d62915812a0d7400e0002c.web-security-academy.net/'/>

- Add a query string 
`/?test`

- Look again --> the query is reflected inside that `href`:

<link rel="canonical" href='https://0adf008f04d62915812a0d7400e0002c.web-security-academy.net/?test'/>

*Angle brackets are escaped, so I can't close this tag and start a new one. But I'm sitting inside a single-quoted attribute value and quotes are NOT filtered, so I can break out of the `href` value and bolt on extra attributes of my own. The task literally says "inject an attribute".*

*A `<link>` tag has no onload/onerror that fires on its own, and there's no element to click. The clue is the simulated keypresses: ALT+SHIFT+X, CTRL+ALT+X, Alt+X. Those are the browser shortcuts for an element's `accesskey`. If I give the tag `accesskey="X"` plus an `onclick` handler, pressing the access-key combo activates the element and fires `onclick` --> `alert`. (This only works in Chrome, which is why the lab notes that.)*

*So I need to inject two attributes: `accesskey='X'` and `onclick='alert(1)'`. I use single quotes to open/close each injected value, reusing the tag's existing quotes to keep the markup valid.*


- Load that URL --> press the access key (Chrome: CTRL+ALT+X on Mac) --> the link is activated --> `onclick` fires --> `alert(1)` pops

**Answer:**

Query string appended to the home page URL:

```
?'accesskey='X'onclick='alert(1)
```
