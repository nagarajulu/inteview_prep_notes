
# HTTPS - Security

### SSL Handshake process

[Reference](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm)

Following are key steps in HTTPS Communication
 - SSL or TLS handshake
 - Server - Digital (SSL) certificate verification; client uses public key in the certificate to encrypt the messages. Server will need to use its private key to decrypt the message. Note that this is same private key which is used when generating certificate request with certificate authority (CA). 
 - Secure communication using chosen cryptographic algorithms during handshake

For 2-ways SSL Handshake, server needs the client's SSL/Digital certificate. server would use this to encrypt back info, client would use its private key to decrypt.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI5MzU4MTgxNF19
-->