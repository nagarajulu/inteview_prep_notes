# Storage and Retrieval

* Understand how databases internally store, and retrieve data

## Why its important?

* Helps make better design decisions when choosing databases for your application.
* Understand difference between storage engines used in
  * OLTP - Transactional Processing Workloads.
  * OLAP - Analytics.

* Understand storage engines used in RDBMS / NoSQL databases.
  * *Log-structured* storage engines
  * *Page-oriented* storage engines

## Data Structures that Power Your Database

#### Log
* word *log* is often referred to application logs, where an application outputs text that describes what is happening. 
* Log per book: an append only sequence of records. 
    * it doesn't have to human readable;
    * it might be binary and intended only for other programs to read.

#### Index

* Additional data structure that is derived from primary data
* Boosts performance of read queries.
* Slows down writes.
* Choose indexes wisely considering query patterns.

### Hash Indexes

* Use in-memory hash map as index for key-value data stores. 
* Append key/value pair data to a file, and keep the byte offset of inserted data in hash map.
* When reading, use the hash map to find offset in data file, seek to that location, and read the value.


* Bitcask (default storage engine in Riak) uses this approach.
* Bitcask offers high-performance reads and writes subject to requirement that all keys must fit in RAM.
* Well suited for situations where value is updated frequently for each key.

* Log is broken into segments at regular intervals, and written to disk. This is to avoid running out of disk space. Subsequent writes happen to new segment.
* Compaction is performed on old segments to throw away duplicate keys, and keep only recent update.
* Merging can also be performed with compaction in background thread. When merging is complete, read is switched to using new segments.

#### Issues during implementation

* *File Format*: CSV is not best format for log; Use binary with length in bytes, followed by raw string itself.

* *Deleting records*: Append a special deletion record (called *tombstone*) to data file. tombstone tells merging process to ignore previous values for this key.

* *Crash recovery*: on restart, all in-memory hash maps are lost. Though we can restore segments by reading entire file from disk, it is time consuming. Bitcask speeds up recovery by taking snapshot of each segment's hash map on disk, which can be loaded into memory quickly.

* *Partially written records*: database could crash at any time, including half-way through appending a record to log. Bitcask files includes checksums, allowing such corrupted parts of log to be detected and ignored.

* *Concurrency control*: Writes are appended to log in strictly sequential order, implemented using one writer thread. since data file segments are append-only and immutable, reads can happen concurrently by multiple threads.

#### Limitations

* Must be kept fully in memory (RAM). 
* Range queries are not efficient. eg: can't easily scan over all keys between kitty00001 and kitty99999, as you have to look up keys individually.

### SSTables

* Sorted String Tables
* Similar to Hash Indexes, except that it requies key-value pairs to be sorted by key. 
* Also requires that each merged segment file to have only one occurrence of each key.

#### Advantages of SSTables over HashIndexes

* Merging segments is simple and efficient. Follows Merge-sort like approach. 
* No need to keep all keys in in-memory index. Can be Sparse. Keeping one key per KB of segment file is sufficient, as KB can be scanned quick.
* Since read requests scans over range of keys, it could be compacted/stored to save disk space, and reduce I/O.

#### Constructing and Maintaining SSTables

* When a write comes in, write to in-memory balanced tree data structure (eg: Red Black Tree, AVL Tree)
  * In-memory tree is also called *memtable*
* When *memtable* gets bigger than threshold (eg: few MB), write to disk as SSTable file.
  * new SSTable becomes most recent segment of database.
  * while SSTable is being written to disk, writes can continue to new memtable instance.
* For serving reads, first try to find key in memtable, then most recent on-disk segment, and next older etc.
* From time to time, run merging and compaction process in background to combine segment files and discard overwritten or deleted values.

* **Issue** if database is crashed, the most recent writes (that are in memtable and not written to disk) are lost.
  * To overcome this, can keep seperate log on disk to which every write is immediately appended.
  * Log is not in sorted order, and its only purpose is to restore memtable after a crash.
  * When memtable is written to disk, log can be discarded.

#### LSM Trees vs SSTables

* Above process of creating SSTables makes this a Log Structured Merge-Tree  (LSM Tree)
* Storage engines that work on principle of merging and compacting sorted files are called LSM storage engines. 

* LSM Trees are used in LevelDB by Google, RocksDB by Facebook is extension of LevelDB.
* LevelDB can also be used in Riak (as an alternative to Bitcask)
* LSM Storage engines used in Cassandra, and HBase as well.

#### Performance Optimizations

* LSM tree algorithm can be slow, when looking up keys that do not exist in database. 
  * have to check memtable, then segments all the way back to oldest, before making sure key doesn't  exist

* To overcome this, storage engines use *Bloom Filters*.

* **Bloom filter** is memory-efficient data structure for approximating the contents of a set.
  * It can tell you if a key doesn't exist in the database.
  * Saves many unnecessary disk reads for nonexistent keys.

### B-trees

* Used by most RDBMS, and few NoSQL databases
* Like SSTables, B-trees keeps key-value pairs sorted by key

#### How B-trees Index works
* B-trees break database into fixed-size blocks or pages traditionally 4KB in size (sometimes bigger)
* Resembles closely to underlying h/w, as disks also arranged in fixed-size blocks
* Read or write one page at time
* One page is designated as root of tree
* each page contains range of keys, and references to child pages
* leaf pages contain data inline or have references to actual pages with data.
* When you want to look up key in index, you start at root, and keep going down like in BSTs to leaf.

#### Branching factor
* Number of references to child pages in one of B-tree is called branching factor.
* Usually branching factor is typical hundreds in number.

#### Updates and Adds/deletes
* For updates, search for leaf page containing key, change value in that page, and write page back to disk.
* For Adds
  * find a page whose key range encompasses the key, add key to that page.
  * if there isn't enough space, the page is split into 2, and parent page is updated with the new child page ranges.

#### Complexity, Size
* B-tree with *n* keys always has a depth of O(logn)
* Most databases can fit into a B-tree with 3 or 4 levels deep
* 4-level tree of 4KB pages with a branching factor of 500 can store up to 256TB.

#### Making B-trees reliable

* **Crashes**
  * *Write-ahead log* (WAL, aka *redo* log): append-only file to which every B-tree modification must be applied before it can be applied to pages of tree itself.
  * When DB comes up after crash, this log is used to restore B-tree back to resilient state.
* **Concurrency**
  * protect tree's data structures with *latches* (lightweight locks)

#### Optimizations to B-trees

* Instead of overwriting pages and maintaining WAL for crash recovery, some databases uses copy-on-write scheme.
  * modified page is written to different location, new version is created, and pointing to new location.
  * this approach also useful for concurrency.

* Save pages by not storing entire key, but abbrevating it. With this, more keys can be packed in page, thus increasing *branching factor* and also fewer levels.

* Have additional pointers to left and write in leaf pages.

* B-tree variants like *fractal trees* borrow log-structured ideas to reduce disk seeks.

### Compating B-Trees and LSM Trees


| B-Trees          | LSM-Trees      |
| :-------------  |:-------------|
| Faster for reads | faster for writes |
| Modifies in place | Append-only | 
| write data 2 times for every key: one to write-ahead log, and one to tree page itself | rewrite data multiple times due to compaction/merging of SSTables. This effect of resulting one write to multiple on disk is called *write-amplification* |

### Advantages of LSM Trees

* LSM-trees can be compressed better
* Sustain higher write throughput than B-trees, as they have sometimes have lower *write-amplification*
* Lower-storage overheads as they do leveled compaction/merging.

### Downsides of LSM-trees

* compaction process could interfer with ongoing reads/writes.
* if write throughput is high, and compaction is not configured properly, you will eventually run out of disk space.
* LSM stores same keys in multiple places unlike B-trees that have only one place. this makes B-trees more favorable to transaction based workload.

### Other Indexing Structures

#### Primary key index

* index configured on primary key of relational db.

#### Secondary index

* index configured on other columns of relational db table. likely have duplicate values

#### Clustered vs Non-clustured indexes

* Clustured index stores all row contents along with index itself. (eg: MySQL Innodb engine)
* Non-clustered index stores only references to data within an index.

#### Covering indexes (aka Index with Included columns)

* compromise between Clustered and Non-clustered
* stores some of tables columns data with an index.
* allows some queries to be answered by this index alone.

#### Multi-column indexes

* *concatenated index* combines several fields into one key by appending one column with another. (eg: telephone lastname/firstname)

* 2-D location (latitude/longitude) can be translated into single number using space-filling curve, and use B-tree type index. (eg: R-trees)

### in-memory databases

* Memcached - caching use only, data lost on restarts.
* VoltDB, MemSQL, Oracle TimesTen - in-memory relational databases.
* RAMCloud - in-memory key-value store with durability (LSM approach for data in memory as well as disk)
* Redis, Couchbase - weak durability by writing to disk asynchronously.

## OLTP vs OLAP

* OLTP - Online transaction processing
* OLAP - Online analytical processing.

### Stars and Snowflakes - Schemas for Analytics

* star schema (or dimensional modeling) is used in data warehouses.

* Center of schema is called *fact-table* 
  * eg: represents sales, click of user in website
* *fact tables* have few attributes, and other as foreign-key references to other tables called *dimension tables*.

* Name *star schema* comes from the fact that when table relationships are analyzed.

* *Snowfake schema* - in this, dimensions are further broken down into subdimensions.

### Column Oriented Storage

* useful in datawarehousing, analytical databases that have TBs of data.

* idea is to store all column data of table together

* *Apache Parquet* is columnar storage format that supports document data model, based on Google's Dremel.

#### Column Compression

* Column oriented storage often lends better to compression

* *Bitmap encoding* is technique largely effective in dataware houses.

### Aggregation - Data Cubes and Materialized Views

* data warehouse queries often involve an aggregate function eg: COUNT, SUM, AVG, MIN, MAX in SQL.

* Cubes (additional tables) that store pre-computed values upfront for widely used aggregations, and used on read.

* These data cubes or OLAP cubes are creating using *Materialized Views* 

* *Materialized view* is like *Virtual View* but has an actual copy of query results, written to disk.

* When underlying data changes, a materialized view needs to be updated, and database can do that automatically. but this makes writes more expensive.
