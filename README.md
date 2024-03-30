# Document the SSIS Package Configuration: 
- Opened SQl Server management studio
- Created table in SQL server management studio with respective col names
- As per instructions everything is defaulted to string
- Created SSIS project in visual studio
- Under Control Flow create a pipe line to load data
- Create Flat file src object
- Create connection manager connecting object to csv file
- create a derived column object for cleaning data

# Rationale for Data Cleaning Methods: 
# Challenges Encountered: 
I was having issues just connecting in general
![encryptError](./photos/certError.png)
Sometimes the sln is right in front of your face
![sln](./photos/solCert.png)

Seems to be a cast error in from string to int for UID and age
![castErr](./photos/castErr.png)

Looking over instructions again it looks like that is intnetional

Setting up everything to play with eachother was a serious headache

Getting error on converting flat file to numeric due to "potential loss of data"
Replace method does not seem to be doing anything with an empty string

After hours of debugging it turns out when you compare some to "" you're looking for the empty string
But if the table has "" it is the string literal ""
# Adaptability of the Package: 
# Resources used:
- https://learn.microsoft.com/en-us/sql/integration-services/sql-server-integration-services?view=sql-server-ver16
- https://www.youtube.com/watch?v=0ikNnenDyNw
- https://www.youtube.com/watch?v=iPre6qerdrY
- https://stackoverflow.com/a/30851840
# Questions
- How are you supposed to deal with empty strings, checking if len = 2 seems counterproductive
- What is the point of a Data converter if you need to change the field type in the source anyways?
- You have the instruction i.	Removing duplicates: Identify and eliminate duplicate records based on specific criteria or key columns.
In the data set there are clear duplicate records, in actual prod, would these not be additional transactions by the user?