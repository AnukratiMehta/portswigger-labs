# Lab: SQL injection attack, listing the database contents on non-Oracle databases

**End Goal:** Log in as the ```administrator``` user.


**Analysis**

*The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.*

*The SQL injection vulnerability is in the product category filter so let's add ' after the category to see if the page breaks*

https://0a49009504d2c5b580a12bc8003b0054.web-security-academy.net/filter?category=Accessories%27

*The page broke. So the injection works. Now check how many columns are there.*

' ORDER BY 1--

*Error. Why though? Seems like a MySQL quirk.*

*On MySQL, the -- comment is only recognised if it's followed by a space (or a newline).*

' ORDER BY 1-- -

*No error*

' ORDER BY 2-- -

*No error*

' ORDER BY 3-- -

*Error detected so there are only 2 columns. Check the cheat sheet FOR . Shows this for MySQL:*

SELECT @@version

**Answer:** 

Accessories' UNION SELECT @@version, NULL-- -

