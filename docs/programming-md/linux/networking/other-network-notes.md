# Other networking notes

## Internet Protocol (IP)

DSL stands for **Digital Subscriber Line**, a technology that provides high-speed internet access over standard telephone lines.

Ethernet > IP > TCP or UDP > HTTP data, VoIP data, Email data  
Boxes of TCP and UDP hold your data.

Header + payload + trailer

TCP & UDP are in OSI layer 4 (the Transport Layer)

Every computer has an **IP address**. IP by itself refers to the protocol.

Khi deliver đến IP address thì TCP & UDP sẽ có port for different services/application on the server.

TCP và UDP có port numbers separately, range from 0 to 65,535.

## TCP & UDP

TCP - Transmission Control Protocol

- Establishes a connection between sender and receiver before transmitting data, ensuring a reliable and ordered stream of packets.
- Reliable: uses acknowledgments, retransmissions, and error checking to ensure all data arrives correctly and in the right sequence.
- Có flow control.
- Những protocol sử dụng cơ chế của TCP: HTTP, SSH

UDP - User Datagram Protocol

- Connectionless: UDP does not establish a connection before sending data, making it faster and simpler
- Unreliable: UDP does not guarantee delivery or order of packets. Packets may be lost or arrive out of order
- No flow control.
- Faster than TCP, real-time communication.

Other connectionless protocols giống UDP

- DHCP (Dynamic Host Configuration Protocol)
- TFTP (Trivial File Transfer Protocol)
