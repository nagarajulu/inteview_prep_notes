## System Design Questions

There are questions I was asked during system design.

### Find the first non-repeating word in file

**Problem:  Given a text file of size say 100 GB? Task is to find first non repeating word in this file?  
*constraints*: You can traverse the file only once.**


 - We would store every word in a map using key-value pairs <word, firstIndex>. 
 - We fill the map as we traverse the file. If we are adding a word that already has an entry, we set firstIndex to a NULL value. 
 - After traversing the file, we go over each inserted word and find the smallest firstIndex that is not NULL. 
 - This is a space complexity of O(k) where k is the number of unique words in the file, and a time complexity of O(n) where n is the file size.  
 - If they are English words, which are easily below a million, our space complexity will be below 10 megabytes, assuming 10 characters per word.

### Find top 10 errors by count in distributed logs ( about 100GB or more i.e. can't fit in one machine)
Assume log format for each entry is
"timestamp, app-name, http status code, error message etc..."

### Design short-term website/app which is used for 3 days, and is meant to accept donations from millions of volunteers across country for a charity. Assume 3rd party vendor does actual payment processing.

### Design a system capable of tracking bad request down quickly

Say your system are expected to serve 100 billion requests per day on average,
one day, a user complains that his client shows that one request failed,
he wants you to help track the bad request down.
How should you design such a system that is able to give the result to the user as quickly as possible.

Note: You should consider scalability, imagine that you could possibly have hundreds of servers spread across tens of datacenters.

### How to handle large log data?

Image there is a website generates user access log in Terabytes scale every day. Each log contains many different information such as username, visiting time, location, page visited, and etc.
what's your solution if we want to only store certain info(for example, user, login time, location) to a sql database. How to optimize the process.

### Given log file of the past 24 hrs, return the top 3 errors, given time of 5 mins window.

Log file format: goal is to return error codes.
Log file size: 10 million entries.

Timestamp, error code, some other entries