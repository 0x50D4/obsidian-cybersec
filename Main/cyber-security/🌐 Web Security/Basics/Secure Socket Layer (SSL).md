It provides security to the data transferred between the browser and the server. Ensures that all data passed between the two is secure and not modified. SSL works on the 6th layer of the [[OSI Model]]

Secure socket layer protocols:
- SSL record protocol
- Handshake protocol
- Change-cipher spec protocol
- Alert protocol

### SSL Record Protocol
It provides two services: 
1. Confidentiality
2. Message integrity



In the SSL protocol application data is divided into fragments, that is compressed and then encrypted by MAC (Message Authentication Code), generated by algorithms like SHA and MD5. After the encryption is done, the SSL header is appended to the data.

### Handshake protocol

Used to establish sessions. Allows the client and server to authenticate.

Phase 1: Hello messages with cipher suite and protocol version exchange.
Phase 2: Server sends certificate and Server-key-exchange, ends phase 2 with server-hello-end packet
Phase 3: Client replies by sending 


#notfinished