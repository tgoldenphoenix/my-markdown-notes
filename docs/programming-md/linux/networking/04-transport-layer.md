# 04-Transport Layer

UDP & TCP

It is above network layer & below Application Layer.

Đọc tới chapter 3.2 Multiplexing

## Introduction

To simplify terminology, we refer to the transport-layer packet as a **segment**.
Của network layer là **datagram**, nhưng đừng nhầm với User Datagram Protocol (UDP).  
We believe that it is less confusing to refer to both TCP and UDP packets as segments, and reserve the term datagram for the network-layer packet.

Recall that the transport layer lies just above the network layer in the protocol stack. Whereas a transport-layer protocol provides logical communication **between processes** running on different hosts, a network-layer protocol provides logical-communication between hosts. This distinction is subtle but important.

Transport-layer protocols are implemented in the end systems but not in network routers.

You must know the destination IP address for both UDP and TCP. TCP guarantee your packet arrive whereas UDC do not guarantee anything.

## Transport-Layer Multiplexing & Demultiplexing

- The job of delivering the data in a transport-layer segment to the correct socket at the receiving host is called **demultiplexing**.
- The job of gathering data chunks at the source host from different sockets, encapsulating each data chunk with header information (that will later be used in demultiplexing) to create segments, and passing the segments to the network layer is called **multiplexing**.

When the transport layer in your computer receives data from the network layer below, it needs to direct the received data to the appropriate process.

A process (as part of a network application) can have one or more sockets, doors through which data passes from the network to the process and through which data passes from the process to the network. Thus, as shown in Figure 3.2, the transport layer in the receiving host does not actually deliver data directly to a process, but instead to an intermediary socket.

Because at any given time there can be more than one socket in the receiving host, each socket has a unique identifier. The format of the identifier depends on whether the socket is a UDP or a TCP socket, as we’ll discuss shortly.

Although we have introduced mul-tiplexing and demultiplexing in the context of the Internet transport protocols, it’s important to realize that they are concerns whenever a single protocol at one layer (at the transport layer or elsewhere) is used by multiple protocols at the next higher layer.

The unique identifier for sockets are the **port number**. Each socket in the host could be assigned a port number, and when a segment arrives at the host, the transport layer examines the destination port number in the segment and directs the segment to the corresponding socket. The segment’s data then passes through the socket into the attached process. As we’ll see, this is basically how UDP does it. However, we’ll also see that multiplexing/ demultiplexing in TCP is yet more subtle.

Each port number is a 16-bit number, ranging from 0 to 65535. The port numbers ranging from 0 to 1023 are called **well-known port numbers**  and are restricted, which means that they are reserved for use by well-known application protocols such as HTTP (which uses port number 80) and FTP (which uses port number 21).

## Connectionless Transport: UDP

At the very least, the transport layer has to provide a multiplexing/demultiplexing service in order to pass data between the network layer and the correct application-level process.

UDP, defined in [RFC 768], does just about as little as a transport protocol can do. Aside from the multiplexing/demultiplexing function and some light error checking, it adds nothing to IP.

These two minimal transport-layer services—process-to-process data delivery and error checking—are the only two services that UDP provides! In particular, like IP, UDP is an unreliable service—it does not guarantee that data sent by one process will arrive intact (or at all!) to the destination process.

DNS is an example of an application-layer protocol that typically uses UDP.

Some applications are better suited for UDP than TCP.

 The TCP segment has 20 bytes of header over-head in every segment, whereas UDP has only 8 bytes of overhead.

