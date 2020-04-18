# System Design - Algorithms, Techniques

| Problem | Technique |  Summary | 
| ----------- | ----------- | ------------ |
| Partioning | Consistent Hashing | Incremental Scalability | 

## 1. Consistent Hashing

Special kind of hashing such that when a hash table is resized, only K/n keys need to be remapped on average, where
* K is the number of keys, 
* n is the number of slots

### Need for consistent hashing
* Eg: Load balancing ‘n’ cache machines  --> Common way is to put object ‘obj’ in machine → hash (obj) % n
* This will not work if cache machine is added or removed

* Consistent hashing maps objects to the same cache machine, as far as possible. 
  * when a cache machine is *added*, it takes its share of objects from all the other cache machines 
  * when it is *removed*, its objects are shared among the remaining machines

* very useful strategy for distributed systems (eg: distributed caching, distributed databases, distributed servers etc)

## 2. Bloom Filters

A *Bloom Filter* is a space-efficient probabilistic data structure.

### Uses / purpose

Main purpose of Bloom Filter is to eliminate false negatives in the dataset. Following are possible outcomes of querying an item using Bloom Filter.

* *False Positive* - which means it may or may not exist in the dataset
* *False Negative* - it means that your query item definitely doesn't exist in the data set

### How it works

Uses multiple hashing functions (unrelated to each other) on inputs, and sets corresponding bits in data structure. Essentially this data structure is simply an array of bits sufficient enough for data set in consideration. 

Apply each of the hash functions to input, get int, and set corresponding bit in bloom filter array.

## 3. Merkle Trees

For verifying the datasets quickly for integrity using multiple levels of hashing.

## 4. Sloppy Quorums and Hinted Handoff

For high availbility in writes.

