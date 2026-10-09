# Lab: Reflected XSS into HTML context with nothing encoded

**End Goal:** Perform a cross-site scripting attack that calls the ```alert``` function.

**Analysis**

*There is a search bar there. Add alert function in it.*


**Answer:** 

<script>alert("Bad stuff happening here!")</script>



