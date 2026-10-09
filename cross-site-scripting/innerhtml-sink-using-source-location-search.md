# Lab: DOM XSS in innerHTML sink using source location.search

**End Goal:** Perform a cross-site scripting attack that calls the ```alert``` function.

**Analysis**

*There is a search bar there. Add alert function in it.*

<script>alert("Bad stuff happening here!")</script>

*This just gave me: 0 search results for '<script>alert("Bad stuff happening here!")</script>'*

*document.getElementbyId.innerHTML doesn't execute the script so we need to use a different payload.*

**Answer:** 
*I put a number in the image src that will throw an error, which, in turn, will execute the alert function.*

<img src=1 onerror=alert(1)>




