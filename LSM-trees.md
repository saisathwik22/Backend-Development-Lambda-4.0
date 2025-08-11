## LSM Trees - Log Structured Merge Trees
- it's a data structure used by Cassandra, DynamoDB, ScyllaDB, Couch Base, Big Table .... etc
- with current setup of a file (append only) and a hashmap we can make a few changes and get LSM.
- The append only file we have is called `WAL - Write Ahead Log` file in HDD.
- WAL is used by Kafka, NoSql, Sql.

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
