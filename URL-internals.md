# What happens when we type URL in browser ?

<img width="804" height="596" alt="image" src="https://github.com/user-attachments/assets/c6d577b9-5858-41e6-8d36-bd81195f7b60" />

- Static IP Address : more expensive because they are permanent.
- Dynamic IP Address : cheaper, every time you reconnect you get new IP address.
- Client should know server's IP Address to raise a request.
- Once client receives response from DNS it caches for future communication with DNS to avoid multiple DNS access requests.

## Protocol:
- refers to rules setup inorder for a task to be completed.
- rules for 2 machines to communicate, different for different type of communication.

- SMTP (Simple mail transfer protocol) : email transfer between machines.
- FTP (File transfer protocol) : file transfer between machines.
- HTML data transfer : HTTP (Hyper Text Transfer protocol)

- OSI model & TCP/IP network models define steps for most of protocols to work when one machine has to communicate with other.

<img width="770" height="670" alt="image" src="https://github.com/user-attachments/assets/f66f2d08-d22a-4d80-a729-3c47d7eeb857" />

### Application Layer: 
- Exists on final apps and intercats with end user.
- Logic of how this layer needs to work is implemented on these apps.
- Protocols like HTTP, HTTPs, SMTP, FTP, WebRTC, VOIPN all of these exists and are controlled in the application layer.
- Most of codes of project are written.
  
### Transport Layer:
- Data collected on application layer is passed here.
- Transport layer logic exists on OS of your machine.
- Any protocol you are following on application layer can be classified in one of the 2 categories (reliable/unreliable)
  - Reliable protocols: ensures data is delivered accurately and in correct order. Error detection, retransmission, ACKs. More overhead, more complex, network congestion.
  - ex: HTTP, websocket, FTP
  - Unreliable protocols: No guarantee of successful data delivery, correct order, error free. Fast and lightweight and efficient. Simple design, connectionless, no error correction.
  - ex: UDP, IP, NTP, VOIP
- If a request from application layer is coming from reliable protocol, then that protocol depends on TCP protocol implemented on transport layer and for non reliable ones they depend on UDP
- Transport layer has only 2 protocols : UDP, TCP
- Data is converted to segments if TCP followed.
- Data is converted to datagrams if UDP followed.

### Network Layer: 
- Data collected on transport layer is passed here.
- It exists within software and hardware of network devices like routers, network drivers, and hardwares.
- logic of network layer exists in OS kernel.
- KERNEL : core of an OS, acting as main interface between hardware and software managing resources and facilitatin communication between them.
### NIC card (network interface card/network adapter/network interface controller):
- hardware component that allows computer to connect to a network enabling data transmission and reception.
### How to route your packets/datagrams?
- routers use routing tables to make decision about which path to take based on destination IP address.
- These routing tables are static/dynamic
- routing tables contain information about network paths and best way to forward packets.
- Static routing tables: manually configured.
- dynamic routing tables: learn three routing protocols.

- Nodejs has NET modules
- it can create TCP client using `net.createConnection()`
- this function takes server's IP and port address and a callback function that executes when connection established.
- callback function allows you to send data to server using socket.

### Datalink Layer:
- error detection is major use case.
- most of logic written in NIC, OS drivers, WiFi routers.

### Physical Layer:
- actual final physical wires.


<img width="1326" height="981" alt="image" src="https://github.com/user-attachments/assets/a4d209c9-6d20-48dc-a85e-fa5fa24a3da6" />
