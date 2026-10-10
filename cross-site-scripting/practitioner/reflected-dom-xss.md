# Lab: Reflected DOM XSS

**End Goal:** Create an injection that calls the `alert()` function.

**Analysis**

*Search for a test word.*

test

*Open DevTools --> Network --> find the search request. The response is JSON with my word reflected in it:*

{"searchTerm":"test","results":[]}

*So if I break out of the "searchTerm" string I can inject code. Tried closing the string with a double quote. Doesn't work. The app escapes my " into \", so it stays inside the string. Get around the escaping by sending my own backslash first.*

**Answer:** 

\"-alert(1)}//




