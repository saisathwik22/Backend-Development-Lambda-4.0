# Hyper Text Transfer Protocol - HTTP
- primary communication mechanism between machines.
- Application layer network layer
- It is reliable protocol hence dependent on TCP protocol in transport layer
- It is set of rules that generally govern how can you send hyper text over a network between 2 machines

`Hyper Text` : its a text/document that contains hyperlinks to other text/documents.

- It will define how machine will format, read and transmit hyper text from one machine to other machine
- Throughout years, http has developed to become more general purpose application layer protocol.
- Its now capable of not just transmitting hyper text but mostly any kind of data images, text, json.... etc

<img width="1244" height="347" alt="image" src="https://github.com/user-attachments/assets/85f75c0f-bb5e-4856-8299-fe4b813bb340" />

- setup TCP connection for http to work using 3-Way Handshake
- after connection setup, send http req
- both http request and response has multiple components and the protocol defines what should happen in those components.

```
protocol://ipaddress:PORT
```
- http (1.0) : for every new request, new TCP connection to be setup, No support of persistant connections.
- http (1.1) : persistant connections | on same TCP connection multiple http request can be sent and multiple http response can be recieved | responses can be streamed in parts (chunk by chunk)
- http (2) : performance improvements
- http (3) : uses `quic` instead of TCP for faster, secure and reliable web experience.

```
http request contains
- URL of server to which we want to communicate
- request headers with extra meta data about request, more like key:value pairs.
- request method, an enum that can be used to identify nature of request (GET, POST, DELETE, PATCH)
- request body, payload of information that we can send along with request.
http response contains
- response body, response of server
- response header
- response code - numeric value, which can be used to identify result of request.
```
---

# API : Application Programming Interface
- API is a mechanism with which outer world will be able to interact with out software
- JAVA streams API (set of functions)
- functions implemented by JAVA providing
- facility to access stream related functionality of JAVA
- when client and server are on same system/machine, then they do not need network interaction.

- server contains business logic for client to interact with server
- server should expose some kind of an API for all this to happen, network (TCP) is needed.
- This is for "distributed systems"

#### API writing ways:
- SOAP, REST, gRPC, graphQL (meta)
- microsoft uses OData API : Open Data (best way of REST) (2007)
- most widely used : REST, gRPC, graphQL
