# Understanding Database Internals

### Initial Requirements:
- fast reads, fast writes, durability, unstructured data, handle a few petabytes of data

### Relational DBMS:
- don't support unstructured data
- schema predefined in RDBMS
- for every row we know exact size and data types.
- SQL is neither a language nor a database.
- MySQL and PgSQL are RDBMS

### What will happen during "reads" ?
- assuming data is stored by id, because of fixed schema and defined size, DB knows exactly where next record is.
### Why?
- Because from starting byte of one row, we can add 84 bytes and reach next row.
- Even for update on a specific column of row, we just need to go to specific location and update
- Since updates won't even change size of data hence allocated memory space is same.

### How data orientation on HDD will look like for a key value based NoSQL ?

```
sample data = {
  iphone11 : 4.8,
  iphone12 : 4.7,
  macbook : 4.9,
  magsafe : 3.2,
}
assume every character/value is of 1B size
for example, "iphone11" has 8 characters so it is of 8B, "4.8" is of 1B
```
- HDD representation assuming continuous writes.
- assume every cell is 1B of storage
- for storing value like "4.8" store it in four cells with one empty cell after "4", ".", "8" cells
- after every key leave 1B as seperator (**)

<img width="765" height="742" alt="image" src="https://github.com/user-attachments/assets/1f13cc9a-8664-462e-bf45-773b1b2e5dd1" />


### How will "updates" will work ?
- If we are updating the data, and when no space is left then, don't override existing data.
- split data and store remaining data some where else on the disk, and maintain pointer for this splitted data.

```
Now, we want to update [macbook : 4.9] to [macbook : "apple"]
before update,
[m] [a] [c] [b] [o] [o] [k] [**] [4] [.] [9] [empty cell] [m] [a] [g] [s] [a] [f] [e] [**] [3] [.] [2] [empty cell]
since number of characters in apple is more than 4.9
after update,
[m] [a] [c] [b] [o] [o] [k] [**] [a] [p] [p] [pointer for data "le"] [m] [a] [g] [s] [a] [f] [e] [**] [3] [.] [2] [empty cell]

since there are 5 chars in "apple" and we have only 4 cells, store a, p, p in 3 cells.
now store "le" somewhere in disk, and create a pointer to location where "le" is stored.
now store that pointer in the cell.

This is horrific approach wrt HDD
The pointer can lead data to some other sector, jumping across sectors randomly is not recommended.
HDD are optimized for sequential access, not random access.

So this pointer (Linked List) type approach is BAD !!
```

- when update arise, donot over ride.
- append new data in HDD
- for reads, do linear search from end, TC: O(n) not fast.

```
to update macbook : 4.9 to macbook : "apple",
instead of replacing 4.9 with apple, insert another entry

after update,
macbook : 4.9
macbook : apple
```
- This is similar to Dijkstra's Algorithm
- In heap, if we find any new short distance, we don't update existing entry, we insert new entry into the heap
- And heap sorts it according to user's condition.

### How can we optimize this ?
- LRU cache can be the idea.
- for LRU cache to find a node fast, we maintain hashmap of data and address node.
- similarily, what if we maintain a hashmap of data and address on disk where latest data is stored?

- Maintain a hashmap in RAM, store in `key:addr` format.
- addr: address of memory location on HDD where value of key is stored.
- Fast search : O(1) avg

<img width="1349" height="984" alt="image" src="https://github.com/user-attachments/assets/3f36d234-109c-43bf-bb48-e46bf31a3bbb" />

- RAM read and write faster, HDD read and write slower.
- reads on RAM 100x faster than HDD
- writes on RAM 1e5x faster than HDD

### Why are we even writing in HDD if it's that slow and only write in RAM hashmap?
- Storing only in RAM lacks durability
- value can be big complex object leading to fast filling of RAM if we only rely on that.

1. Old data is never deleted so how to handle this evergoing data problem?
2. for billions of unique keys RAM won't be able to fit the entries. (ideally 1e9 keys ~ 1B is 1GB)
3. what about support for partitioning and sharding ? (because file can become huge)
4. If system crashes, then data in RAM will be lost, what about that?
5. using data on HDD we can recover data and reconstruct(using WAL file) RAM offset hashmap
