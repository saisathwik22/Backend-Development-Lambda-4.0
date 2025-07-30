# Backend System Architecture

## Software Engineering Life Cycle (SDLC)

<img width="980" height="530" alt="image" src="https://github.com/user-attachments/assets/4b49d67a-a00f-4d8b-919a-d0805ed7ddab" />

### Consider Social Media Application Design

- PC's and Mobiles have lot of competancy differences, where PC is more capable.
- Mobile client keeps payloads less in memory.
- Maintaining multiple TCP connections is relatively easier and will take very less load on browser client/laptop related client compared to mobiles.
- Apps are always made in accordance with browser client.
- Any backend applications have 2 major components, Computing Capabilities and Storage Capabilities

- Browser Client raises a request "CREATE POST" using some networking protocol.
- Client sends some data{...} to machine regarding request.
- Code logic runs on RAM, it becomes process in RAM, this process collects the request.
- Now, create post data{...} needs to be stored.

<img width="656" height="551" alt="image" src="https://github.com/user-attachments/assets/19e4d0e3-9ac1-4e43-978e-3c4bdc2004c7" />

- Caching Data : Store data such that it can be accessed faster than usual
- Dynamic Programming is nothing but caching.

- `Latency` : total delay time happening since start of a request till time we recieve response.
- Total turnaround time, PING
- Factors contributing to Latency
  1. External : network delay
  2. Internal : time taken to access HDD/SSD, CPU time(microseconds)
- Cross country call - Global Network Latency ~ 150ms
- Same country - Different ISP ~ 20ms to 100ms
- Small region call ~ 10ms to 50ms
- HDD are 20-30 times slower than SSD

- SSD has `read` and `write` operations, where write is expensive than read operations in 99% case
- Two types of read : `sequential reads` and `random reads`

- How HDD store data ?
  - It uses magnetism, magnetising tiny regions on spinning disks to represent 1s and 0s which are read by read/write head.
  - `sequential reads` are FASTER than `random reads`
  - Sequential read is done in adjacent blocks and sectors
- Sequential Read:
  - easy to read and faster
  - reads in a list where all data you want is present.
- Random Read:
  - group of lists where data you want is spread across multiple lists.
  - tough and time taking and painful reading, Slower.
  - data not present in adjacent sectors and blocks.
  - random iteration of head, time taking(delay)
  - mechanical movement of head.
  - lesser the head moves, faster the access.

- Databases are Row-oriented (optimized for row storage), faster to read 1 complete row
- Databases are Column-oriented (optimized for column storage), faster to read 1 complete column.

- Reading 1 Mb of data (SSD) : sequential manner (1ms) | random access (10ms)

- `Apache Cassandra` : uses column family, not purely column-oriented DB, wide column store.

- Way of storing data determines latency, time to access (sequential faster | random slower)
- sequential reading 20 ms | random reading 200 ms | for HDD
- sequential reading 0.25 ms | random reading 2.5 ms | for RAM | faster than SSD
- sequential reading 1ms | random reading 10 ms | SSD

- In MySQL, `table_open_cache` determines maximum number of tables the server can keep open concurrently
- performace optimisation, reduces overhead of repeatedly opening and closing tables.
- When a table needs to be opened and cache is full, MySQL closes least recently used table.
- If all tables in cache are in use, MySQL temporarily extends cache and closes table as they become idle.
- `Table cache` : memory area that stores information about open tables.

`Cache memory` : L1, L2, L3 | represents different proximity to CPU, reduce workload on CPU.

- `LEVEL 1 (L1)` :
  - smallest and fastest
  - embedded directly into CPU, operate at same speed as CPU
  - 2 parts : one for storing instructions (Li1) | one for storing data (L1d) | division supports different fetch bandwidth used by processors.
  - size of L1 is 2 KB to 64 KB
- `LEVEL 2 (L2)` :
  - large and slightly slow compared to L1 cache.
  - always closer to CPU than main memory.
  - can be shared among multiple cores or exclusive single core depending on CPU architecture.
  - size of L2 is 256 KB to 512 KB.
- `LEVEL 3 (L3)` :
  - larger than L1 and L2, slower
  - located outside CPU and shared by all cores
  - plays important role in data sharing and inter core communications.
  - size of L3 is 1 MB to 8 MB
 
`Thrashing (Operating Systems)` :
- when system spends on excess amount of time swapping data between physical memory (RAM) and virtual memory (disk storage) due to high memory demand and low available resources leading to performace degradation.
- occurs when system spends more time swapping pages than actually performing using tasks
- caused by high number of page faults.

`Thundering Herd Problem` : 
- occurs when large number of processes/threads waiting for resource/event are awakened.
- parallely leading to surge of requests and potential system overload as they all compete to access same resource.

- Now `SCALING` the system is required.
1. `Vertical Scaling`:
   - try to increase capacity of computing resource you have
   - there's an upper limit
   - ex: hire 15+ years exp SDE instead of intern.
2. `Horizontal Scaling`:
   - choose multiple good computing resouce instead of increasing capacity.
   - ex: hire multiple 5 years exp SDE instead of 15+ years exp SDE and intern | not as pro as 15+ yoe SDE not as noob as intern, Decent.
  
<img width="1675" height="813" alt="image" src="https://github.com/user-attachments/assets/878ded6c-eb2f-42dd-a118-e0a19b06d525" />


### Stateful System
- configure `Load Balancer` in such a way that if a request is made it knows which server it should be redirected to.
- ex : Chat Application system
