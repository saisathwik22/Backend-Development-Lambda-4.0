## Client Server Architecture
- client is any process running on a machine that raises a request for particular task
- server is also a process running on a machine capable of accepting request from client, process it and send a respone to client.
- for every request, server used to create a new process, each of these process correspond to individual requests.
- if CPU is single core, context switching happens.

- creating brand new process --> "Expensive Task"
- for running many process --> "Good RAM and CPU rqd"
- it's not scalable.

### Frequency of a processor:
- number of clock cycles a CPU completed per second, measured in GHz.
- higher GHz value ---> faster processor (giga - 1 billion)

Clock Cycle:
- single operation performed by CPU.
- GHz value tells you how many of these cycles happen per second.
- 1 giga = 10^9 bytes.

# Threads:
- lightweight process
- creation of thread is not heavy process, wrt process creation.
- Threads dont have all procedures of process creation.
- Threads are spawned by processes.
- for creation of threads, we dont need to create heap, code/text area, global var.
- instead these components are shared from main process.
- but, it requires creation of stack, program_counter, registers.
- thread dont have own unique pid, every thread within a process shares same pid as process it belongs to.
- each thread have own unique tid.
- thread id of initial thread === pid of entire process.
- threads apart from first/initial thread, has unique tid.
```
process ID + thread ID ---->  Task ID
```
# How CPU handles multiplie Processes ?
- modern OS creates illusion of simultaneous execution through context switching.
- Scheduler rapidly switches CPU b/w different processes allowing each process to make progress over time. [Concurrency]

# Why do processes block for I/O ?
- not all operations require continuous CPU execution.
- operations like reading a file from disk, waiting for user input, causes process to block and wait.
- To prevent CPU being idle, OS switches execution to other process allowing useful work to continue while waiting for I/O operations to complete.

# Problem with Blocking requests:
- if multiple clients send request simultaneously a problem arises.
- 1st request gets accepted and processed, other request must wait until first request is done.
- CPU sits idle during disk read operations, wasting valuable time.
- This is "blocking problem" where useful computational time is wasted due to sequential execution.
## Traditional Solution : Creating new process for each request
### Approach : multiprocess model
- main processor accepts a request, spawns new process to handle request, listener remains free to accept new request.
- This improves Concurrency, but drawbacks are:
  - Creating new process for each request = Inefficient
  - High memory usage
  - spawning = slow (OS must allocate memory, rgst, file desc)
  - inter process communication overhead
  - if processes need to share data they require IPC mechanisms which add complexity and latency.
  - limited scalability
### Better Approach : using threads instead of processes
- threads allow concurrent execution within same process.
- Thread is lightweight unit of execution, share same memory space but maintain their own execution state.
- each thread has own program_counter, own CPU registers, own stack
- threads within same process share code section, global var, heap memory, file descriptors and I/O handles.

## How threads solve blocking problem
- with a multithreaded server
  - listener process accepts a request.
  - instead of spawning a new process, creates a new thread.
  - while one thread waits for dist I/O, other thread can process a different request.
- This ensures CPU is used efficiently, reducing idle time, improving response time.

## Memory Management of Threads:
- each thread ==> own stack preventing data corruption.
- shared heap memory requires synchronization to avoid race conditions.
- Mutexes and semaphores help prevent multiple thread from modifying shared data simultaneously.
## OS level implementation of threads:
- most OS implement threads within PCB
- Linux represents both processes and threads using task structure.
- each process starts with a main thread, additional threads are created dynamically.
- OS schedules threads instead of entire processes improving efficiency.
## Threads vs Functions
- function is reusable block of code, but it doesnot execute independently.
- thread is an independent execution until that runs in parallel with other threads.
- mutliple threads can execute same function parallely without conflicts.
## Threads are more efficient than processes:
- faster context switching coz they share same memory
- lower memory usage no duplications.
- better resource sharing (directly access without IPC)
- higher scalability (more handle capacity)
- reduced memory overhead.

- Threads offer powerful alternative to traditional multiprocess concurrency on single core processors especially.
- Efficient CPU usage.
- prevent idle time during I/O operations.
- reduce memory overhead.
- allow fast context switching.
- multithreading is complex but preferred for scalable, high performant applications (synch challenges).
