# Lab: Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped

**End Goal:** Perform a cross-site scripting attack that calls the `alert` function inside the template string.

**Analysis**

- Open Burp --> Proxy tab --> Open Browser --> Access lab
- Search for `test` --> view source --> my input is reflected inside a JS template literal(backticks) in an inline `<script>` block:

<script>
var message = `1 search results for 'test'`;
document.getElementById('searchMessage').innerText = message;
</script>


- In Repeater, probe the search term to see what's filtered:
- `<` `>` --> HTML-encoded
- `'` `"` --> HTML-encoded
- `` ` `` --> escaped --> can't close the template literal
- `\` --> escaped

*Every break-out character is handled, so I can't close the string or the script. But the string is a template literal, and that's the whole point: template literals evaluate `${ ... }` substitutions in place. The filter leaves `$`, `{` and `}` alone, so I don't need to break out at all. I just drop an expression inside the string and the engine runs it for me.*


**Answer:**

${alert(1)}

