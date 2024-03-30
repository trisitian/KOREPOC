# Document the SSIS Package Configuration: 
## Component Break Down
### Start (Flat File)
This is the initial CSV containing examples provided in instructions
### Clean Data (Derived Column)
This component cleans data and replaces null values, it removes quotes from fields that
are not meant to be strings. It replaces null values with defualts that make sense for the type.
### Convert To Proper (Data Conversion)
This component simply converts each field to its proper type represented in DB
### Seperate Unknown Entries (Conditional Split)
In The Clean Data step, if a UID is missing, it is replaced with 0 as a special value
Theese entries are then seperated at this component to be calculated and stores seperately
### Remove Duplicates (Sort)
A sort funciton sorting records by UID's and removing duplicates based on UID
### Server Destination
Where the data is stored
### Tally Uknown Dollars (Aggregate)
Of all records that did not have a UID, we sum their purchase totals
### Add Missing Cols (Derived Column)
Tacks back on default values in other columns not including purchase totals
### Prepare Data (Data Conversion)
Cleans Data to match schema
### Missing Data Statistics
A Seperate CSV file Containing outliers above
## SQL Server management Studio
- Opened SQl Server management studio
- Created table in SQL server management studio with respective col names (defined in spec)
- PK is set to UID and all values are typed intuitively

# Rationale for Data Cleaning Methods: 
I have made some basic choices for cleaning
I have made the assumption that the most important piece of information is purchase total 
No field in specific seems important enough to make the choice that if missing, the whole record should be dropped
So I have opted defualt values for each
I will outline defualts for each field
### FullName
If this is missing it is defualted to "Null User" 
### Email
If this is missing it is defaulted to "UnknowEmail@temp.com"
### Age
If this is missing it is defaulted to 18
### UserID
If this is missing it is defualted to 0, the assumption is made that 0 will not be a normal UID
(and if it is I would choose something to be known not a possible assignment) This is marked specially becuase if a UID
is missing, then the PK is missing and so we cannot do any sort of interpolation based off other data. This value is then used
later in the pipeline to collect other meaningfull data, and then it is discarded. 
### Registration Date and Last Login Date
If theese peices are missing they default to 1900-01-01
### Purchase Total
I could not tell if this field was a "make or break" for keeping the record. Currently it defualts to 0
It was a toss up between this and discarding the record, in a work environment I would clarify this with you



# Challenges Encountered: 
I have never used SSIS, and in fact had not even heard of it before approaching this assignment
Setting up Microsoft SQL server management studio was causing issues as I kept receiving an error about
"The certificate not coming from a trusted source" which turned out to be a check box that I had forgotten 
to click that was right in front of my face. I had a similar issue in visual studio. 

Actually getting the SSIS toolbox was a headache, I downloaded the extension and I just couldn't put two and two together
I attribute alot of this to not using Visual Studio in some time and so it was growing pains with the IDE

Typing fields was extremely frustrating. Any time I tried to pass components through the pipeline I would get some 
error about "Potential loss of data" and looking all over the place I was configuring the Flat file itself, the server, 
It took me forever to realise this had to do with SSIS not playing well with andything that isn't a string if physical quotes are present

The other huge roadblock was implementing the logic for cleaning the data, The REPLACE funciton was not working if a string was empty ("")
It turns out this is becuase the empty quotes are taken as a string literal and so the string was never empty and so REPLACE never actually triggered. 

Other minor challenges popped up here and there trying to figure out what components to use where and the basics of whats in the SSIS toolbox
# Adaptability of the Package: 
This package is Null proof (assuming that the csv will always have "" for empty data), it has logic to deal with it
There is no limitations on scaling up or down as the process is configured to run one record at a time
This package is duplicate friendly and will automatically discard any duplicate entries. 

# Resources used:
- https://learn.microsoft.com/en-us/sql/integration-services/sql-server-integration-services?view=sql-server-ver16
- https://www.youtube.com/watch?v=0ikNnenDyNw
- https://www.youtube.com/watch?v=iPre6qerdrY
- https://stackoverflow.com/a/30851840
- a million other videos that forgot to record
# Questions
- How are you supposed to deal with empty strings, checking if len = 2 seems counterproductive
- What is the point of a Data converter if you need to change the field type in the source anyways?
- You have the instruction i.	Removing duplicates: Identify and eliminate duplicate records based on specific criteria or key columns.
In the data set there are clear duplicate records, in actual prod, would these not be additional transactions by the user?