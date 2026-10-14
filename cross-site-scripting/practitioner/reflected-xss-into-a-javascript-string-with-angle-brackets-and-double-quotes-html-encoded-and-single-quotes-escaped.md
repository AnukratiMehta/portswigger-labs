# Lab: Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped

**End Goal:** Break out of the JavaScript string and call the `alert` function.

**Analysis**

- Open Burp --> Proxy tab --> Open Browser --> Access lab
- Search for `test` --> view source --> my input is reflected inside a JS string in an inline `<script>` block:

<script>
var searchTerms = 'test';
document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
</script>

- In Repeater, probe the search term to see what's filtered:
  - `<` `>` --> HTML-encoded --> can't break out with `</script>` like the previous lab
  - `"` --> HTML-encoded --> no help in this single-quoted string anyway
  - `'` --> escaped to `\'` --> can't close the string directly
  - `\` --> **NOT** escaped --> this is the gap

*The HTML escape kills the `</script>` trick, so I have to break the JS string itself. The server escapes my single quote to `\'`, but it leaves my backslashes alone.*

- Search payload: `\'-alert(1)//`

**Answer:**

\'-alert(1)//

