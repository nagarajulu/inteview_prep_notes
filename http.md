
# HTTP and HTTPS Protocols

## Key Notes

 - Stateless Protocol
 - Sits in Application Layer of OSI Networking Reference Layer
 - Runs on top of TCP/IP, default port 80
 - HTTPS runs on default port 443. HTTPS has additional TLS or SSL layer in between HTTP and TCP/IP.

### HTTP Connections

 - HTTP Connection is established between Client (IP, Port) and Server
   (IP, Port).
 
 - HTTP 1.0: All connections are closed after a single transaction. 
 - HTTP 1.1 Supports **persistent** connections i.e. long-lived connections, they are kept open until client explicitly asks to close. 
	 - They are default in HTTP 1.1.
	 - Client can explicitly set `'Connection: close'` header if server can
   close connection after sending back response.
   
 - Browsers/Clients also leverage technique '**parallel**' connections,
   to minimize the network delays.

## HTTP Headers

### General Headers

 - All HTTP 1.1 clients are required to accept `Transfer-Encoding: chunked` header. this is used to break the response into smaller parts; Allows streaming of server responses instead of one big payload to client.
 - Via Header is used in TRACE command, and is updated by all intermediate proxies/gateways/routers with its IP
 
 ### Entity Headers
 - Content-* headers (Content-Type, Content-Length, Content-Encoding, Content-Language, Content-Location etc) specify structure, encoding, and size of message. 
	 - some of these headers are applicable when the relevant entity is present in body.
Expires, Last-Modified are used to track when the message expires, or was last modified.
Custom headers can be created by client, and are treated as entity headers by the HTT.


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzEyODMzOTEzLDIyMTMzMTU0LC00MjcyMD
Q5OV19
-->