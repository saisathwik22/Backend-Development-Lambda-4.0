## Apache Web Server:
- launched in 1995
- free and open source cross-platform web server.
- effort to develop and maintain open source HTTP server for modern OS including UNIX and windows.
- provide secure efficient and extensible server that provides HTTP services in sync with current HTTP standards.
- it's the project of The Apache Software Foundation

## Web Server v/s Application Server:
### Web Server:
- static content
- response to simple requests from browsers
- uses HTTP but also supports FTP and SMTP
- static content process and delivery
- might act as proxy for application server.
### Application Server:
- dynamic content
- responds to complex requests from application, browsers, mobile apps.
- supports many protocols including HTTP, CGI, msg bus.
- execute business logic.
- transition mgmt, security and scalability.

PHP + MySQL + Linux => LAMP Stack => use Apache Web server

![image](https://github.com/user-attachments/assets/3a838682-2abc-4cd0-8bc6-fbee1046a74b)

- Early Apache Server used to create new process for every request leading to more memory consumption.
- Now, they have improved by using threading approach, introduced a single thread per request instead of whole process.

## NGNIX :
- worked on non-blocking, event-driven architecture
- Ngnix ensures that if we have x no. of cores then x no. of worker processes will be created to maximize efficiency and performance.
- Each worker process is a single threaded processes.
- whenever new client connection comes up, 1 worker process creates a connection with client.
- every worker process emits a new event whenever new connection is established.
- worker process does not wait for blocking request's response, instead starts listening to new request.
- when you recieve a req --> register that as an event --> if its blocking event --> don't wait for it to complete --> handle in diff way.

## High I/O vs High Processing:
- Nodejs good for high i/o, but not for high processing
- High processing operations need high CPU power.
- High processing task ----> training ML model ----> need high quality CPU.
- High i/o ----> single threading.
- Some operations in nodejs works on multiple threads, some on single threads.
