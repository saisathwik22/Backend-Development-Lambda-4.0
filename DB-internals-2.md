## LSM Trees - Log Structured Merge Trees
- it's a data structure used by Cassandra, DynamoDB, ScyllaDB, Couch Base, Big Table .... etc
- with current setup of a file (append only) and a hashmap we can make a few changes and get LSM.
- The append only file we have is called `WAL - Write Ahead Log` file in HDD.
- WAL is used by Kafka, NoSql, SQL.

---

### Why RDBMS uses WAL ?
- to speed up writes RDBMS uses WAL
### Why?
- RDBMS writes data on HDD. To improve reads they support indexes (B+ trees)
`FACT` : on HDD sequential read is way faster than random read because read happens in consecutive blocks.
- At every write on SQL database we update data (1 disk seek)
- update & rebalance B+ tree index (logn disk seek)
- total logn on disk
- So instead of rebalancing on every write SQL uses WAL.
- In this WAL it stores these "recent writes" and then whenever batch is full, then we move it from WAL to B+ tree index
- B+ tree index allows easy modification, but updating B+ trees is heavy task.
- `WAL and B+ tree` together they try to optimize your writes.
- If lot of writes are coming, some recent writes you can store in WAL and in one shot you can upadte B+ tree because re-indexing a B+ tree is heavy
- WAL also used for master slave replication.
- MySQL - further optimization => Change Buffer (similar to WAL)

`Change Buffer` : helps reduce performance cost of updating indexes by "caching" changes.
- Whenever you perform CRUD that affects an index and the type of change is recorded in change buffer.
- Next time you read that index with a query, MySQL reads change buffer as a kind of supplement to full index.
- Change buffer only used for secondary indexs, not for primary key/unique key index.

---

## WAL : Write Ahead Logging
- ensures data integrity
- changes to data files must be written only after those changes have been logged.
- In case of crash, we can recover the DB using log.
- application of changes can be redone using WAL
- This is called `Roll-forward recovery` (REDO)
- Journaled file systems improve boot speed after crash.

`NOTE`: we can only do insert at end and get operation on WAL file.

### Why NoSQL use WAL ?
- NoSql uses WAL as a part of data storage mostly facilitated by LSM.
We will use WAL in following way:
1. we will use WAL to keep apending data.
2. we maintain a hashmap to track for a key, what was "latest offset" for that key in WAL.

- Procedure to remove duplicated from WAL : `COMPACTION`

- How about we run periodic job to remove old entries.
- but with this deletion will be happening in between file, so to reclaim that space we might need to shift remaining data.
- Also when we delete, then we won't be able to take new writes and reads until delete finishes.
- Also RAM hashmap will undergo lot of updates
- Because WAL can be heavy so this deletion needs to handle file chunk by chunk as complete file is heavy and big.

- Now if we are alraedy suggesting to read file chunk by chunk why not maintain multiple smaller size WAL file?
- Also in hashmap now instead of just storing key and offset we store key, WAL file and offset.

<img width="1227" height="497" alt="image" src="https://github.com/user-attachments/assets/e80d79f0-5dec-40af-b521-4859926c7bae" />

- Smaller WAL files, easy to manage

<img width="1283" height="646" alt="image" src="https://github.com/user-attachments/assets/5077b59a-35a6-4247-b1e9-219b09ed981e" />

- HashMap : Store latest data of keys

<img width="645" height="927" alt="image" src="https://github.com/user-attachments/assets/fc150159-3ada-4e0b-9742-c5bfb16c351e" />

`READ` : get(key) - find key in hashmap in Avg Tc O(1) RAM
                  - get file offset O(1) disk
        put(key, value) - append to latest WAL O(1) disk
                        - set in hashmap O(1) avg tc
                        - if WAL file size > limit => create new WAL file - O(1)

- So, now both get and put are efficient
`NOTE`: all "writes" are happening in active WAL file
        reads can happen in any WAL File
- Because of this, `COMPACTION` process can work on old file.
- It will not interfere with writes and writes won't be blocked.
- Reads might be impacted.

### Can we improve further ?
- Now our latest WAL file is only 100mb
- So how about we keep latest WAL files in disk & RAM both,
- `MEMTABLE`: refers to hashmap prepared for the latest active WAL file / refers to RAM version of active WAL.

### Why we want to keep it ?
- on disk updating is issue, as size keeps on changing.
- on disk pointers are bad as random access is very slow.
- So memtable will not keep a duplicate as it is also a hashmap.
- Also we can agree that data on memtable is always latest as its representing active WAL.
- Now if we read, and key is in memtable then read is going to memory, hence it is very fast.
- So when we write we update memtable & also append to WAL (to ensure durability)
`Active WAL` : durability | `Memtable` : faster reads

### Now what if while appending to WAL we don't immediately write to file offset hashmap ?
- This would be good as latest data is already coming from memtable.

### So when should we add data to hashmap offset ?
- Once our WAL file (active) is full then we dump it's data in offset hashmap.

### Can we optimize what we do once a WAL file is full ?
1. When active WAL is full, we create a new file.
2. But what if we dump active WAL in a new file in a way that we store data in arranged form?
3. So we use memtable (unique values) & take all key value, sort it in memory and dump these unique keys in sorted form to a new file.
4. This file will act as newly created dump of last active WAL but in sorted form with unique keys.
5. Instead of keeping exact WAL file, we store this new file as it has lesser data (as only unique keys from memtable is added) & sorted data (searches are faster).
6. This new file is called as `SST : Sorted Set Table` | keys are sorted | all values are unique (set)

### Steps for write when WAL file is full
1. flush memtable on disk as a new SST
2. SST will be sorted by key
3. update offset hashmap
4. create a new WAL file & dump old one (as data is in new SST in persistant function)

### Should we create memtable ?
- No, we can keep an upper limit to size of memtable
- once we reach limit we can have LRU or LFU or FIFO eviction
- `LRU`: Least recently used | `LFU`: Least frequently used

### Is the problem of duplicates solved ?
- In a single SS table we dont have duplicates, but between 2 SS tables, duplicates can be there.

### Is the problem of sharding solved ? YES
- these SSTables don't need to be on same server.
- easily replicate it.
- reads are faster & writes also faster.
- create copies of SST asynchronously behind the scenes, in case if HDD is tampered.

### Reads are faster, HOW?
- As memtable is acting as a cache, & if the `hit ration` is good, data reads will be faster.

- Now as majority of reads are gonna be handled by memtable & it can definitely contain latest data.
- Compaction can happen on WAL files without hampering reads.

`COMPACTION` : procedure to remove duplicates | operates on background on SST
- SS tables are small, so we can easily load 2-3 tables in RAM.
- Say, we can take 2 SS tables, load in RAM & remove duplicates and create a new de_duplicated WAL
- very similar to merging 2 sorted arrays.
- just like in merge sort, we can combine 2 SS tables into 1 with only unique entries.
- to know which SSTable has new table we can use file name.
- while merging we keep latest entry and discard old one.

<img width="952" height="972" alt="image" src="https://github.com/user-attachments/assets/279aaab7-ec94-46f2-a250-593ca121a6d4" />

### Can we now eliminate offset hashmap ?
- Currently we use offset hashmap to find an entry
- this hashmap can become too large in size
- if we know which SS tables to search we can use binary search to get data.

### How we know correct SStable to search for ?
- because of compaction number of SST will be less (but of large size) as we get rid of lower SST
- because of tree structure, number of sst is approx logn
- we should reduce these logn disk reads as they are expensive.

---

## Sparse Index :
- lets divide our WAL file in small chunks such that each chunk can go on a single block of HDD
- Now if we know the right chunk, then in one HDD read we get complete block & we can do binary search on it.
- So for each SST, we maintain sparse index.
- This sparse index contains starting value of each block.

- SS tables are immutable, we can only create it and search on it | No CRUD

### So how to delete a key?
- make new entry of key with a `SENTINAL VALUE`

`SENTINAL VALUE` : special value to mark end of a sequence/data structure
- Also to check if a key is present in LSM or not.
- we should not go to all SST instead use `BLOOM FILTERS`

`BLOOM FILTERS` : probabilistic, space efficient data structure.
- test if an element is member of a set of values or not
