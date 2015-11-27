---
layout: post
title:  Real time web - base technologies
date:   2015-11-18 08:43:00
categories: http architecture communication
tags: websockets server-sent-events long-polling comet BOSH
---

In a standard HTTP request-response scenario a client opens a connection, sends a HTTP request to the server (for example a HTTP GET request), then receives a HTTP response back and the server closes the connection once the response is fully sent/received. The initiative always comes from a client when the client requests all the data.

After AJAX there is a need of server side push to support things like notifications, collaboration, chat, comments, any stuff which you need in real time. The simple solution to this is let the server initiate the connection but that will crush the beliefs of http as server will have to maintain states and also just think about security. So there are many workarounds exists out there to simulate this thing but with client initiating the connection.

## Comet
Comet is a web application model in which a long-held HTTP request allows a web server to push data to a browser, without the browser explicitly requesting it. Comet is an umbrella term, encompassing multiple techniques for achieving this interaction. Comet is known by several other names, including Ajax Push, Reverse Ajax, Two-way-web, HTTP Streaming, and HTTP server push among others.

### 1. Polling

_Procedure_:

- With polling a client repeatedly sends new requests to a server.
- If the server has no new data, then it send appropriate indication and closes the connection.
- The client then waits a bit and sends another request after some time (after one second, for example).

### 2. Long-polling

_Procedure_:

- With long-polling a client sends a request to a server.
- If the server has no new data, it just holds the connection open and waits until data is available.
- Once the server has data (message) for the client, it uses the connection and sends it back to the client.
- Then the connection is closed.

### 3. Server-Sent events
SSE is similar to the long-polling mechanism, except it does not send only one message per connection.

_Procedure_:

- The client sends a request and server holds a connection until a new message is ready
- then it sends the message back to the client while still keeping the connection open so that it can be used for another message once it becomes available.
- Once a new message is ready, it is sent back to the client on the same initial connection.
- Client processes the messages sent back from the server individually without closing the connection after processing each message.

So, SSE typically reuses one connection for more messages (called events). SSE also defines a dedicated media type that describes a simple format of individual events sent from the server to the client. Also not that SSE is not duplex.

### 4. WebSocket 
WebSocket technology is different from previous technologies as it provides a real full duplex connection.

_Procedure_:

- The initiator is again a client which sends a request to a server with a special HTTP header that informs the server that the HTTP connection may be "upgraded" to a full duplex TCP/IP WebSocket connection.
- If server supports WebSocket, it may choose to do so.
- Once a WebSocket connection is established, it can be used for bi-directional communication between the client and the server.
- Both client and server can then send data to the other party at will whenever it is needed.

The communication on the new WebSocket connection is no longer based on HTTP protocol and can be used for example for for online gaming or any other applications that require fast exchange of small chunks of data in flowing in both directions. To know more about why websockets 2.0 (using TCP) performs better than 1.0 (only http) follow [this](http://stackoverflow.com/questions/14703627/websockets-protocol-vs-http).

## BOSH
Bidirectional-streams Over Synchronous HTTP (BOSH) is a transport protocol that emulates a bidirectional stream between two entities (such as a client and a server) by using multiple synchronous HTTP request/response pairs without requiring the use of polling or asynchronous chunking. BOSH achieves this by avoiding HTTP polling, yet it does so without resorting to chunked HTTP responses as is done in the technique known as Comet.

_Procedure_:

- For `push`, a BOSH client starts an HTTP request, but the server postpones sending a reply until it has data to send.
- After receiving a reply, the client immediately makes another request on the same HTTP connection, so the server can always send data to the client without waiting for the client to poll.
- If, while waiting for a reply, the client needs to send data to the server, it opens a second HTTP connection.
- There are at most two HTTP connections open at a time, one on which the server can send data as a reply and one on which the client can send data as a POST.

## Real Time Tools
There are many tools evolved over these base technologies. I will be covering some of them soon but you can find a list of available tools at [REAL-TIME WEB TECHNOLOGIES GUIDE](http://www.leggetter.co.uk/real-time-web-technologies-guide/).

## References

1. [Comet](https://en.wikipedia.org/wiki/Comet_(programming))
2. [BOSH](https://en.wikipedia.org/wiki/BOSH)
3. [What are Server-Sent Events](https://jersey.java.net/documentation/latest/sse.html#d0e11362)
4. [REAL-TIME WEB TECHNOLOGIES GUIDE](http://www.leggetter.co.uk/real-time-web-technologies-guide/)
5. [What are Long-Polling, Websockets, Server-Sent Events (SSE) and Comet?](http://stackoverflow.com/questions/11077857/what-are-long-polling-websockets-server-sent-events-sse-and-comet)
6. [WebSockets protocol vs HTTP](http://stackoverflow.com/questions/14703627/websockets-protocol-vs-http)