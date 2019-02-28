
# HTTPS - Security

### SSL Handshake process

[Reference](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm)

Following are key steps in HTTPS Communication
 - SSL or TLS handshake
 - Server - Digital (SSL) certificate verification; client uses public key in the certificate to encrypt the messages. Server will need to use its private key to decrypt the message. Note that this is same private key which is used when generating certificate request with certificate authority (CA). 
 - Secure communication using chosen cryptographic algorithms during handshake

For 2-ways SSL Handshake, server needs the client's SSL/Digital certificate. server would use this to encrypt back info, client would use its private key to decrypt.

Refer [Two-way-SSL-handshake-process](http://www.cheat-sheets.org/saved-copy/Ssl_handshake_with_two_way_authentication_with_certificates-1.pdf) 

RNC - Random Number Client, RNS - Random Number Server


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExODc5MjcwNTksLTI5MzU4MTgxNF19
-->