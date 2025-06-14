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
