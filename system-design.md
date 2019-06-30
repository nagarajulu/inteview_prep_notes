## System Design Questions

### Find the first non-repeating word in file

Ans: 
We would store every word in a map using key-value pairs <word, firstIndex>. We fill the map as we traverse the file. If we are adding a word that already has an entry, we set firstIndex to a NULL value. After traversing the file, we go over each inserted word and find the smallest firstIndex that is not NULL. 

This is a space complexity of O(k) where k is the number of unique words in the file, and a time complexity of O(n) where n is the file size.  

If they are English words, which are easily below a million, our space complexity will be below 10 megabytes, assuming 10 characters per word.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY2NTcyNTkyMF19
-->