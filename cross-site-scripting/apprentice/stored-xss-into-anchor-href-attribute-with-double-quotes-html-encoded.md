# Lab: Stored XSS into anchor href attribute with double quotes HTML-encoded

**End Goal:** Submit a comment that calls the ```alert``` function when the comment author name is clicked.

**Analysis**

*I filled in all the fields to post a comment and whatever I added in the website field rendered as the link to the author name so the payload will go in the website field.*

<p>
<img src="/resources/images/avatarDefault.svg" class="avatar">                            <a id="author" href="123web">123</a> | 29 September 2026
</p>

*Added to website field but still shows not found on clicking.*

<script>alert(1)</script>

*Still getting Not Found.*


**Answer:** 

javascript:alert(1)






