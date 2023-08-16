OSI stands for Open System Interconnect. It describes networking systems with seven layers.

![[OSI-Model.excalidraw|200|center]]

**Layer 7 - Application**: Provides an interface for the end user on the network. It is not an application itself, but represents the part of the application that communicates. It relies on all layers below/above to complete this process. The data here is presented a way the user can understand.
Examples: HTTP, Telnet, FTP, DNS

**Layer 6 - Presentation**: Takes the data that the application layer sends out, and makes sure it is readable by the application layer of another system. This can include, translation, data compression and encryption. (not all encryption is handled here). It could translate between multiple data formats if needed. 
Examples: SSL, XDR, MIME

**Layer 5 - Session**: It establishes request response communication. When needed it starts a session and authenticates, then sends the first request. After it gets a response, it might send another request, or end the session. Basically, it responds to requests from the presentation layer, and sends service requests to the transport layer. Often communicating through APIs.
Examples: SOCKS, NETBIOS

**Layer 4 - Transport**: This layer manages traffic flow through the network layer, to reduce congestion. This layer ensures that there are no errors, and resend the data if it's corrupted or lost (acknowledgements and re-transmission timers usually). Some firewall security and encryption takes part here. It must communicate the data in a way that it can later be reversed by the receiving device. (multiplexing, de-multiplexing)
Examples: TCP, UDP

**Layer 3 - Network**: Transfers variable lengths of packets from a source to a destination, through one or more networks. Responds to the transport layer, and sends service requests to the data link layer. It deals with organizing the data for transfer and reassembly with datagram encapsulation. This layer also handles the routing protocols.
Examples: IPv4, IPv6

**Layer 2 - Data Link**: Transfers data between network nodes in a Wide Area Network or between nodes on a LAN. It's main job is to break down data into frames and transmit it though the physical layer. Also responsible for some error detection and correction, some addressing so different devices can tell each other apart in a network.
Examples: Ethernet Protocol, Point-to-point, Media Access Control (MAC)

**Layer 1 - Physical Layer**: This layer translates the second layers instructions into physical transmissions.
Examples: Ethernet, USB, Bluetooth


Further learning or notes: [0x00sec](https://0x00sec.org/t/notes-on-the-osi-model/14573), [Comparitech](https://www.comparitech.com/net-admin/osi-model-explained/), 