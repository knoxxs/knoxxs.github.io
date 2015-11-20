---
layout: post
title:  Understanding WebSockets
date:   2015-11-20 08:27:00
categories: programming real-time-web
tags: websockets http real-time tcp
---

Though I (also I think everyone) knows what are webSockets but in this post I am trying to understand how they work.

## Introduction
The web was built around the idea that a client’s job is to request data from a server, and a server’s job is to fulfill those requests. This paradigm went unchallenged for a number of years but with the introduction of AJAX around 2005 many people started to explore the possibilities of making connections between a client and server bidirectional.
 
The biggest thing holding them back was the traditional HTTP model of client initiated transactions. To overcome this a number of different strategies were devised to allow servers to push data to the client. The problem with all of these solutions is that they carry the overhead of HTTP. Every time you make an HTTP request a bunch of headers and cookie data are transferred to the server.

What we really need is a way of creating a persistent, low latency connection that can support transactions initiated by either the client or server.

## How WebSockets Work
WebSockets provide a persistent connection between a client and server that both parties can use to start sending data at any time.

_Process_:

1. The client establishes a WebSocket connection through a process known as the WebSocket handshake. his process starts with the client sending a regular HTTP request to the server. An `Upgrade` header is included in this request that informs the server that the client wishes to establish a WebSocket connection.
    
    ```
    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13
    Origin: http://example.com
    ```
2. If the server supports the WebSocket protocol, it agrees to the upgrade and communicates this through an `Upgrade` header in the response.
    
    ```
    HTTP/1.1 101 Switching Protocols
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
    Sec-WebSocket-Protocol: chat
    ```
3. Now that the handshake is complete the initial HTTP connection is replaced by a WebSocket connection that uses the same underlying TCP/IP connection. At this point either party can starting sending data.


_Note_:

- WebSocket URLs use the ws scheme. There is also wss for secure WebSocket connections which is the equivalent of HTTPS.
- The client sends a `Sec-WebSocket-Key` header containing [base64-encoded](https://www.wikiwand.com/en/Base64) random bytes, and the server replies with a [hash](https://www.wikiwand.com/en/Hash_function) of the key in the `Sec-WebSocket-Accept header`. This is intended to prevent a caching proxy from re-sending a previous WebSocket conversation, and does not provide any authentication, privacy or integrity. The hashing function appends the fixed string `258EAFA5-E914-47DA-95CA-C5AB0DC85B11` (a [GUID](https://www.wikiwand.com/en/GUID)) to the value from `Sec-WebSocket-Key` header (which is not decoded from base64), applies the [SHA-1](https://www.wikiwand.com/en/SHA-1) hashing function, and encodes the result using base64.
- The basic frame protocol is described in RFC 6455; a binary header contains an opcode, length and mask information followed by a block of data. How that data is interpreted is not defined by the specification, nor is the actual value of an optional http header used in the startup handshake: `Sec-WebSocket-Protocol`

With WebSockets you can transfer as much data as you like without incurring the overhead associated with traditional HTTP requests. Data is transferred through a WebSocket as messages, each of which consists of one or more frames containing the data you are sending (the payload). In order to ensure the message can be properly reconstructed when it reaches the client each frame is prefixed with 4-12 bytes of data about the payload. Using this frame-based messaging system helps to reduce the amount of non-payload data that is transferred, leading to significant reductions in latency.

## WebSocket efficiency

- Use case A: 1,000 clients receive 1 message per second: Network throughput is (2 x 1,000) = 2,000 bytes = 16,000 bits per second (0.015 Mbps)
- Use case B: 10,000 clients receive 1 message per second: Network throughput is (2 x 10,000) = 20,000 bytes = 160,000 bits per second (0.153 Mbps)
- Use case C: 100,000 clients receive 1 message per second: Network throughput is (2 x 100,000) = 200,000 bytes = 1,600,000 bits per second (1.526 Mbps)

![Comparison of the unnecessary network throughput overhead between the polling and the WebSocket applications]({{ site.url }}assets/2015/11/understanding-websockets/poll-ws-compare.gif)

For more benchmarking example follow [REST vs WebSocket Comparison and Benchmarks](http://blog.arungupta.me/rest-vs-websocket-comparison-benchmarks/).

## Why is the WebSockets protocol better than HTTP?
WebSockets is better for situations that involve low-latency communication especially for low latency for client to server messages. In real-world HTTP browser connections often several kilobytes of data sent as part of the request (in both directions) including many headers and cookie data. Here is an example of a request/response to using Chrome:

```
Example request (2800 bytes including cookie data, 490 bytes without cookie data):

GET / HTTP/1.1
Host: www.cnn.com
Connection: keep-alive
Cache-Control: no-cache
Pragma: no-cache
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.17 (KHTML, like Gecko) Chrome/24.0.1312.68 Safari/537.17
Accept-Encoding: gzip,deflate,sdch
Accept-Language: en-US,en;q=0.8
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.3
Cookie: [[[2428 byte of cookie data]]]


Example response (355 bytes):

HTTP/1.1 200 OK
Server: nginx
Date: Wed, 13 Feb 2013 18:56:27 GMT
Content-Type: text/html
Transfer-Encoding: chunked
Connection: keep-alive
Set-Cookie: CG=US:TX:Arlington; path=/
Last-Modified: Wed, 13 Feb 2013 18:55:22 GMT
Vary: Accept-Encoding
Cache-Control: max-age=60, private
Expires: Wed, 13 Feb 2013 18:56:54 GMT
Content-Encoding: gzip
```

Both HTTP and WebSockets have equivalent sized initial connection handshakes, but with a WebSocket connection the initial handshake is performed once and then small messages only have 6 bytes of overhead (2 for the header and 4 for the mask value).  The latency overhead is not so much from the size of the headers, but from the logic to parse/handle/store those headers. In addition, the TCP connection setup latency is probably a bigger factor than the size or processing time for each request.

## What are some other related protocols?

1. **HTTP 1.0**: request-response transport protocol layered on TCP. The client makes one full request, the server gives one full response, and then the connection is closed. The request methods (GET, POST, HEAD) have specific transactional meaning for resources on the server.
2. **HTTP 1.1**: maintains the request-response nature of HTTP 1.0, but allows the connection to stay open for multiple full requests/full responses (one response per request). Still has full headers in the request and response but the connection is re-used and not closed. HTTP 1.1 also added some additional request methods (OPTIONS, PUT, DELETE, TRACE, CONNECT) which also have specific transactional meanings. However, as noted in the introduction to the HTTP 2.0 draft proposal, HTTP 1.1 pipelining is not widely deployed so this greatly limits the utility of HTTP 1.1 to solve latency between browsers and servers.
3. **HTTP streaming**: a variety of techniques (multipart/chunked response) that allow the server to send more than one response to a single client request. The W3C is standardizing this as Server-Sent Events using a text/event-stream MIME type. The browser API (which is fairly similar to the WebSocket API) is called the EventSource API.
4. **SPDY**: a Google initiated proposal to extend HTTP using a more efficient wire protocol but maintaining all HTTP semantics (request/response, cookies, encoding). SPDY introduces a new framing format (with length-prefixed frames) and specifies a way to layering HTTP request/response pairs onto the new framing layer. Headers can be compressed and new headers can be sent after the connection has been established. There are real world implementations of SPDY in browsers and servers.
5. **HTTP 2.0**: has similar goals to SPDY: reduce HTTP latency and overhead while preserving HTTP semantics. The current draft is derived from SPDY and defines an upgrade handshake and data framing that is very similar the the WebSocket standard for handshake and framing. An alternate HTTP 2.0 draft proposal (httpbis-speed-mobility) actually uses WebSockets for the transport layer and adds the SPDY multiplexing and HTTP mapping as an WebSocket extension (WebSocket extensions are negotiated during the handshake).
6. **WebRTC/CU-WebRTC**: proposals to allow peer-to-peer connectivity between browsers. This may enable lower average and maximum latency communication because as the underlying transport is SDP/datagram rather than TCP. This allows out-of-order delivery of packets/messages which avoids the TCP issue of latency spikes caused by dropped packets which delay delivery of all subsequent packets (to guarantee in-order delivery).

## Does HTTP/2 make websockets obsolete?
HTTP/2 is not a replacement for websocket but aims to standardize SPDY protocol. In HTTP/2, server-push is used behind the scene to improve resource loading by the client from the browser. As a developer, you don't really care about it during your development. However, with Websocket, the developer is allowed to use API which is able to consume and push message with an unique full-duplex connection.

## What is the fundamental difference between WebSockets and pure TCP?
It's easier to communicate via TCP sockets when you're working within an intranet boundary, since you likely have control over the machines on that network and can open ports suitable for making the TCP connections.

Over the internet, you're communicating with someone else's server on the other end. They are extremely unlikely to have any old socket open for connections. Usually they will have only a few standard ones such as port 80 for HTTP or 443 for HTTPS. So, to communicate with the server you are obliged to connect using one of those ports.

The purpose of web sockets is to allow you to initiate a connection via HTTP, but then negotiate to use the web sockets protocol (assuming the server is capable of doing so) to allow a more "TCP socket"-like communication stream.

## Do WebSockets maintain an open connection for each client?
Each TCP connection in itself consumes very little in terms server resources. Often setting up the connection can be expensive but maintaining an idle connection it is almost free. The first limitation that is usually encountered is the maximum number of file descriptors (sockets consume file descriptors) that can be open simultaneously. This often defaults to 1024 but can easily be configured higher.

HTTP connections, while they don't create open files or consume port numbers for a long period, are more expensive in just about every other way:

- Each HTTP connection carries a lot of baggage that isn't used most of the time: cookies, content type, conetent length, user-agent, server id, date, last-modified, etc. Once a WebSockets connection is established, only the data required by the application needs to be sent back and forth.
- Typically, HTTP servers are configured to log the start and completion of every HTTP request taking up disk and CPU time. It will become standard to log the start and completion of WebSockets data, but while the WebSockets connection doing duplex transfer there won't be any additional logging overhead (except by the application/service if it is designed to do so).
- Typically, interactive applications that use AJAX either continuously poll or use some sort of long-poll mechanism. WebSockets is a much cleaner (and lower resource) way of doing a more event'd model where the server and client notify each other when they have something to report over the existing connection.
- Most of the popular web servers in production have a pool of processes (or threads) for handling HTTP requests. As pressure increases the size of the pool will be increased because each process/thread handles one HTTP request at a time. Each additional process/thread uses more memory and creating new processes/threads is quite a bit more expensive than creating new socket connections (which those process/threads still have to do). Most of the popular WebSockets server frameworks are going the event'd route which tends to scale and perform better.

## Securing webSocket
 
You should strongly prefer the secure `wss://` protocol over the insecure `ws://` transport. Like HTTPS, WSS (WebSockets over SSL/TLS) is encrypted, thus protecting against man-in-the-middle attacks. A variety of attacks against WebSockets become impossible if the transport is secured.

There is one thing to note: if the HTML/JavaScript that opens the secure WebSocket connection comes over non-secure HTTP, the WebSocket connection is still secure, but an attacker might modify the HTML/JavaScript while being sent from the Web server to browser. A HTTP connection isn't protected against man-in-the-middle sniffing or modification.

You can find a bit more on security at [WebSocket Security](https://devcenter.heroku.com/articles/websocket-security).

## References:

1. [An Introduction to WebSockets](http://blog.teamtreehouse.com/an-introduction-to-websockets) 
2. [WebSocket](https://www.wikiwand.com/en/WebSocket)
3. [Using Sec-Websocket-Protocol](http://www.codeproject.com/Articles/716148/Using-Sec-Websocket-Protocol)
4. [WebSockets protocol vs HTTP](http://stackoverflow.com/questions/14703627/websockets-protocol-vs-http)
5. [Does HTTP/2 make websockets obsolete?](http://stackoverflow.com/questions/28582935/does-http-2-make-websockets-obsolete)
6. [What is the fundamental difference between WebSockets and pure TCP?](http://stackoverflow.com/questions/2681267/what-is-the-fundamental-difference-between-websockets-and-pure-tcp)
7. [A Quantum Leap in Scalability for the Web](https://www.websocket.org/quantum.html)
8. [REST vs WebSocket Comparison and Benchmarks](http://blog.arungupta.me/rest-vs-websocket-comparison-benchmarks/)
9. [Do HTML WebSockets maintain an open connection for each client? Does this scale?](http://stackoverflow.com/questions/4852702/do-html-websockets-maintain-an-open-connection-for-each-client-does-this-scale/4881457#4881457)
10. [Websockets - wss on http vs. wss on https](http://stackoverflow.com/questions/26791107/websockets-wss-on-http-vs-wss-on-https)
11. [WebSocket Security](https://devcenter.heroku.com/articles/websocket-security)