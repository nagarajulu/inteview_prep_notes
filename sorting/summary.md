# Sorting

- Most fundamental algorithm in computer science. 
- **"when in doubt, sort"** is one of  first rules of algorithm design. 

## Choosing the sorting algorithm

### How many keys will you be sorting?
| Number of elements | Sorting algorithm |
|--|--|
| n<=100 | insertion sort, shell sort |
| n > 100 | O(nlogn) algorithms: heapsort, mergesort, quicksort |
| n > 5,000,000| External-memory sorting algorithms | 

### Will there be duplicate keys in the data?

 - Sorting works without any issues for unique keys.
 - If there are duplicate keys in data set, and if you need to preserve the **original ordering** of elements after sort, then you need to use **stable** sorting algorithms.
	 - Eg: **merge sort** is stable where as **quick sort** is not.

### What do you know about your data?

 - **Has the data already been partially sorted?**
	 - insertion sort might perform better
- **Do you know the distribution of the keys?**
	- if the keys are uniformly or randomly distributed, then **bucket or distribution sort** works better
- **Are your keys very long or hard to compare?**
	- if the keys are long text, then its better to use short prefix for sorting (say first 10 chars), if any ties can be resolved using full text.
	- Radix sort also can be used.
- **Do i have to worry about disk accesses?**
	- in massive sorting problems, its impossible to keep all data set in memory
		- **external sorting** method is used, since external storage needs to be used.
		- simple external sorting approach is to use **B-tree** and then read keys in in-order traversal to get them in sorted.
		- high-performance external sorting algorithms are based on multiway-mergesort.
			- 

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI2MTcxNTY5NywtNzAwNTIxOTc1LC0yMD
kxODYyMDY4LC0xNDkzMjg4MTI5LDE0MTE2Nzg0M119
-->