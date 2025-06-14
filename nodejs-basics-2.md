- When you run a program ---> program becomes a process.
- program under execution is called process, and it is loaded in RAM.

## Process:
- contains callstack, heap memory, text area which have basic instructions.
- space for static var and memory stuff and global var.
- each process has unique process id (pid), stays in PCB.
- program_counter stays in PCB.
- state of process also monitored.

## Process Control Block (PCB):
- data structure that stores info about a process being managed by an OS.
- stores meta data of process.

  ![image](https://github.com/user-attachments/assets/d4a5ca1c-1275-4c46-a9a1-9c6bd0ac94f8)

## Process Creation:
- memory allocation, process gets loaded in RAM.
- store program code, data, var for process to work.
- PCB is created to keep track of details of program including current state etc.... as shown in diagram.
- pid is assigned (unique).
- when switching b/w processes, OS saves current state of a process (context) and loads state of new process to be executed by CPU.

### CPU internals:
- 1 CPU can have many cores.
- single core CPU is equivalent to company with single employee.
- even with single core CPU, people used to do multitasking (windows XP, 97 ... etc)
- a CORE can execute a single process at a single instance of time.
- program_counter points to exact instruction at that instance of time at which the CPU core is executing.

- FIFO execution structure is not efficient because we cant let other process wait till a process is complete.

## Context Switching:
- Most of the time a very simple CPU can execute 10^8 to 10^9 instructions in 1 second.
- Say we have three processes P1, P2, P3 and we have only 1 second.
- instead of executing only one process in 1 second.
- execute some part of P1, then go to P2, then go to P3 within that 1 second.

execute_some(P1);
wait(P1);
execute_some(P2);
wait(P2);
execute_some(P3);
wait(P3);
again start back with execute_some(P1);

- this all happens within that 1 second.

- Allocate your CPU for a single process.
- previous process wait after executing for some time and net process starts to execute.
- To remember the previous executed process program_counter is used, which stores address of previous executed process.
- state of process : wait/running
- CPU cant be waiting for a process response, it will perform context switching until response is recieved.

- More number of cores, Faster the machine.

context switching working

![image](https://github.com/user-attachments/assets/2bc7a921-104e-444d-9203-6776d82c6365)
