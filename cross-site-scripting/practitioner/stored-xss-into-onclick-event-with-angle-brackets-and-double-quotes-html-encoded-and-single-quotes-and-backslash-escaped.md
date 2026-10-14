# Lab: Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped

**End Goal:** Submit a comment that calls the `alert` function when the comment author name is clicked.

**Analysis**

- Open Burp --> Proxy tab --> Open Browser --> Access lab
- Open a blog post --> fill in the comment form (comment, name, email, website) --> Submit --> go back to the post and look at how the author name is rendered

*The author name becomes a link, and the "website" value I supplied is dropped into both the `href` and an inline `onclick` handler. The onclick looks like:*

```html
<a href="WEBSITE" onclick="var tracker={track(){}};tracker.track('WEBSITE');">NAME</a>
```

*So my input lands inside a single-quoted JavaScript string, inside an HTML attribute. To run code I need to break out of that `'...'` string.*

- Proxy --> HTTP history --> find the POST comment request --> send to Repeater and experiment with the `website` field to see what's filtered:
  - `<` and `>` --> HTML-encoded (`&lt;` / `&gt;`) --> can't inject a new tag
  - `"` --> HTML-encoded (`&quot;`) --> can't break out of the attribute
  - `'` --> escaped to `\'` --> can't close the JS string directly
  - `\` --> escaped to `\\` --> can't neutralise their escaping with my own backslash

*Every obvious break-out char is handled. But there's a gap: the filtering is about literal characters, while the browser HTML-decodes an attribute value BEFORE the JavaScript engine ever sees it.*

*So if I send the HTML entity `&apos;` instead of a literal `'`, the server's escaping leaves it alone (it only escapes real single quotes and backslashes). Then the HTML parser decodes `&apos;` back into a real `'` inside the onclick value, and THAT single quote closes the JS string. The server never saw a quote to escape.*

- My website payload: `http://foo?&apos;-alert(1)-&apos;`

*Breaking it down once it's decoded inside the string `tracker.track('...')`:*

- `http://foo?` --> harmless URL prefix so the value is still a valid-looking link
- `&apos;` --> decodes to `'` --> closes the string: `tracker.track('http://foo?'`
- `-alert(1)-` --> `alert(1)` runs; the `-` operators just glue it to the strings on either side so the expression stays syntactically valid
- `&apos;` --> decodes to `'` --> reopens a string so the trailing `');` is still valid: `-'')`

*Net result when the author name is clicked: `tracker.track('http://foo?'-alert(1)-'')` --> `alert(1)` fires.*

- Post the comment with the website field set to the payload --> go back to the blog post --> click the author name --> the alert pops

**Answer:**

Website field value:

```
http://foo?&apos;-alert(1)-&apos;
```
