# LibUV library :
- responsible for doing all event driven modeling for nodejs.
- Nodejs gives 2 types of version -- Synchronous Version (pure blocking version) and Asynchronous Version
- libUV uses its worker threads to execute heavy task and not block main thread.
- Cryptographic functions are one of those types of tasks which are executed on thread pool.
- LibUV prepares pool of threads.
- libUV uses native async mechanisms which makes node very scalable since only limitation is OS kernel.
- if no other way, it will use thread pool which preserves asynchronicity wrt node's main thread.
- it can still become bottleneck if all threads are busy.
- this brings scalability problems when application are mostly CPU bound.
```
node:worker_threads
```
- module enables use of threads that execute JS in parallel.
- workeres (thread) are useful for performing CPU intensive JS operations.
- they donot help much with i/o intensive work.
- nodejs built_in asynch i/o operations are more efficient than workers can be.

  ![image](https://github.com/user-attachments/assets/2b0a9bcd-5b84-4ced-af6d-53c40840d3fa)

- LibUV powers async processing in nodejs
- provied networking capabilities ----> TCP, DNS resolution, UDP
- internal modules of nodejs use functionality of LibUV library internally.
- majority code of LibUV exists in "C" language (97%) also in "python" (0.1%), shell (0.1%), CMake (0.9%), Makefile (0.7%), M4 (1.4%)
- LibUV => multiplatform support library with focus on asynch i/o, primarily developed for nodejs, but also used by Luvit, Julia, uvloop etc....

## Features of LibUV:
- full featured event loop backed by epoll, kqueue, IOCP, event ports (event specific OS notification system)
- Asynch TCP and UDP sockets.
- Asynch DNS resolutions.
- Asynch file system operations.
- file system events.
- ANSI escape code controlled TTY
- IPC with socket sharing, using UNIX domain sockets or named pipes (windows)
- child processes creation. [child_process module]
- thread pool
- signal handling
- high resolution clock
- threading and synch primitives.

- file i/o and similar operations -----> fs module in nodejs.
- a pool of thread workers, to handle thread specific CPU tasks, by default 4 worker threads.
- full functional event loop (epoll, kqueue, iocp)
### OS specific event notifications system
- epoll ---> linux
- kqueue ----> mac
- iocp ----> windows
- high performance IO mechanisms
- have dedicated event driven mechanisms to do IO ops.

## Handles and requests in LibUV :
- *Handles* are long lived object that can perform asynch operations like TCP setup
- persistant object ---> remain in memory until explicitly closed, created when constant execution/monitoring is required to do.
```
uv_close() ---> to manually close these objects
```
- handle objects
```
uv_loop_t , uv_tcp_t , uv_udp_t ......etc
```
- *Requests* are performed over a handle
- short lived objects (Exist only till duration of operation)
- reading of file/writing of file.
- request objects
```
uv_write_t , uv_fs_t ......etc
```
- a prepare handle gets its callback called once every loop iteration when active.
- A TCP server handle that gets its connection callback called every time there is a new connection.
