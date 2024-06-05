# Client-Server Architecture & HTTP

## Clients & Servers
* Client requests a resource
* Server responds with resource
* These are roles - not technical specs or computer types
* A program can be a client and a server at the same time, making and responding to requests
* EX: During a transaction at a bank, the teller is the server and the person requesting money is the client
* If you have servers on one device making requests on the servers of the other, they then take the client role

## Web Servers
* Processes (running programs) not physical machines
*  Might be running on a laptop, or a Raspberry Pi, or enterprise-grade workstation
*  Listening on a port for incoming requests
*  Send back responses
*  Clients can send a request through the Internet, and the Web Server receives the request

## Internet Communication Protocols
* TCP
* SSL
* BitTorrent
* IMAP
* HTTP
* Our main focus will be HTTP

## Protocol
* Rules for interaction/communication
* Specification, not implementation

### The Knock-Knock Message Protocol
* Joker opens connection with "knock, knock"
* Victim completes handshake with "who's there?"
* Joker transmits identity label: "<IDENTITIY>"
* Victim requests clarification: "<IDENTITY> who?"
* Joker delivers payload: "<PUNCHLINE>"
* Joke is now delivered, close connection. Participants may optionally laugh and/or dodge fists
* This is an example of messaging protocol

## Messaging/ App vs. Transmission
* KnockKnock is an application level protocol
* It specifies the sequence and content of messages
* It does NOT specify how those messages are transmitted/sent
* KnockKnock can be transmitted lots of different ways

# OSI Model
* Open Systems Interconnection Model
* Application is layer 7, and transportation is layer 4
* HTTP is application level, TCP is messaging

# HTTP
* An application-level communications protocol. You might call it a messaging protocol
* Specifies allowable metadata and content of messages
* Does NOT specify how messages are transmitted!
* STATELESS: does not need to remember previous req-res!
* HTTP follows the fundamental notion: ONE REQUEST ONE RESPONSE

## HTTP Protocol
* RFC (Request for Comments) 7230
* By the IETF (Internet Engineering Task Force)
* But a generic messaging protocol
* "HTTP is a generic interface protocol for information systems. It's designed to hide the details of how a service is implemented... independent of the types of resources provided
* Micro services are tiny HTTP servers that expose a REST API. How they're written doesn't matter, we use HTTP to hide the implementation

## HTTP Clients & Servers
* Example Clients: Web browsers, household appliances, mobile apps, command-line programs, communication devices
* Example Servers: Web servers, home automation units, news feeds, traffic cameras, autonomous robots

# HTTP IS NOT A TRANSMISSION PROTOCOL

## HTTP Over Vox
* GET/HTTP 1.1
* HTTP 1.1 200 OK
  
## HTP Over Text
* This would be fine, but arduous

## HTTP Over TCP/IP
* Client makes the request and the server gives the response. The messaging protocol is TCP/IP.
* TCP and IP are separate, but are used together very often

## Remember
* With HTTP, every request gets exactly one (total) response (sometimes a response is broken up into chunks)
* You will never use SEND twice in the same piece of middleware. Wait until the middleware is done running, then SEND again

# HTTP Request
* POST /docs/1/related HTTP/1.1/
* Host: www.test101.com
* Accept: image/gid, image/jpeg, */*
* Accept-Language: en-us
* Accept-Encoding: gzip, deflate
* User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)
* bookId=12345 & author=Nimit
* POST is the verb, /docs/1/related is the URI, bookId is the body and everything else is the headers

## Common Verbs
* GET - "read"
* POST- "create"
* PUT - "update"
* DELETE - "Delete"
* In HTTP these verbs are called methods

# HTTP Response
* HTTP/1.1 200 OK
* Date: Sun, 18 Oct 2009 08:56:53 GMT
* Server: Apache/2.2.14 (Win32)
* Last-Modified: Sat, 20 Nov 2004 07:16:26 GMT
* ETag: "10000000565a5-2c-3e94b66c2e680"
* Accept-Ranges: bytes
* Content-Length: 44
* Connection: close
* Content-Type:text/html
* X-Pad: Avoid browser bug
* <html><body><h1>It Works!</h1></body></html>
The last line is the payload/body

# HTTP Response Codes
* 200-Ok
* 204-No content
* 201-Created
* 304-Cached
* 400-Bad request
* 401-unauthorized
* 404-not found
* 500-server error
