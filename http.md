
# HTTP and HTTPS Protocols

## Key Notes

 - Stateless Protocol
 - Sits in Application Layer of OSI Networking Reference Layer
 - Runs on top of TCP/IP, default port 80
 - HTTPS runs on default port 443. HTTPS has additional T

### HTTP Connections
HTTP Connection is established between Client (IP, Port) and Server (IP, Port).

HTTP 1.0: All connections are closed after a single transaction. 
HTTP 1.1 Supports **persistent** connections and they are default. Client can explicitly set `'Connection: close'` header if server can close connection after sending back response.



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYyNDg5NDg3Ml19
-->