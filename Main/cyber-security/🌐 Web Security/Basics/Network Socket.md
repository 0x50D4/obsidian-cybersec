It acts as an endpoint for delivering and receiving data, it's a two way communication link between two programs on the network. It provides inter-process communication.

Each socket has an address. This socket is composed of an IP address and a port number.
For example: `234.231.3.4:3000`

![[socket-drawing.excalidraw|4000]]

There are 2 types of sockets.
1. **Datagram socket**: This is a type of network with a connection-less. The letters (data) posted into the box are collected and delivered (transmitted) to a letterbox (receiving socket).
2. **Stream socket**: A stream socket is a type of inter-process communication, that provides a connection-oriented, sequenced, and unique flow of data, with well defined mechanisms for creating and destroying communications and detecting errors.



