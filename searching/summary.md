# Searching

## Basic Approaches
Two basic algorithms exists.
 - **Sequential Search**
	 - time complexity - O (n)
	 - generally faster up to 20 elements
 - **Binary Search**
	 - time complexity - O (log n)
	 - input array must be sorted
	 - efficient when n > 100 elements

## Identifying variant of searching algorithm

 - #### Are certain items accessed more often than other ones?
	 - Reduce comparisons in **sequential search** by placing most popular items in front and least popular towards end.
	 - For **binary search** trees, keeping most popular ones to root is quite complicated, that too ensuring BST won't end up like sequential search.
		 - Optimal BST implementation is needed (keeping it balanced)
 - #### Might access frequencies change over time?
	- s
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDc4Mjg5MTg3XX0=
-->