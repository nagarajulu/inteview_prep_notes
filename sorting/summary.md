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

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzk1NDg2NTQxLDE0MTE2Nzg0M119
-->