# Lab: Reflected XSS into HTML context with most tags and attributes blocked

**End Goal:** Perform a cross-site scripting attack that bypasses the WAF and calls the `print()` function.

**Analysis**

- Open Burp --> Proxy tab --> Open Browser --> Access lab
- Proxy --> HTTP history --> find the GET /?search=test request --> right-click it --> Send to Intruder
- Intruder tab --> Clear § --> Replace the search value with angle brackets --> select the spot between < and > and click Add §
- PortSwigger XSS cheat sheet --> Copy tags to clipboard --> Paste into the payload list --> Start Attack
- In the results window, look at the Status (or Length) column --> 200 --> body

*So, body is the allowed tag. Now, to run it, I need an event handloer attribute. Check that again with Intruder.*

- <body §§=1> --> PortSwigger XSS cheat sheet --> Copy events to clipboard --> Paste into the payload list --> Start Attack

*Lots of event handlers got through with 200 status. I have to figure out which will autofire without victim interaction. So, this seems like a good option:*

<body onresize=print()>

*My search payload is "><body onresize=print()>. The "> closes the current tag so my body tag is parsed. But onresize only fires when the frame resizes, and the lab bans user interaction, so I can't just put this in the search bar and resize by hand.*

*The fix: build a page on the exploit server with an iframe. The iframe loads the lab search URL (with my payload in it), and onload resizes the iframe automatically. That resize fires onresize --> print() runs, no clicks.*

*I never touch the search bar myself. My payload lives inside the iframe's src, where it's written URL-encoded so it's valid in a URL:*

" --> %22
> --> %3E
space --> %20

*So the search part of the src reads ?search=%22%3E%3Cbody%20onresize=print()%3E, which is just ?search="><body onresize=print()> encoded. When the iframe loads, the browser runs that search for me.*

- Go to exploit server --> Body field --> paste the iframe with the src to my lab URL --> Store
View exploit --> the print dialog pops --> Deliver exploit to victim

**Answer:** 

<iframe src="https://0a9b008403ab3bd180021cf20059005f.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'></iframe>


