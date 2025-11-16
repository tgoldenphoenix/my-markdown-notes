# Transport Layer

Đọc tới chapter 3.2 Multiplexing

## Introduction

To simplify terminology, we refer to the transport-layer packet as a **segment**.
Của network layer là **datagram**, nhưng đừng nhầm với User Datagram Protocol (UDP).

We believe that it is less confusing to refer to both TCP and UDP packets as segments, and reserve the term datagram for the network-layer packet.

Recall that the transport layer lies just above the network layer in the protocol stack. Whereas a transport-layer protocol provides logical communication **between processes** running on different hosts, a network-layer protocol provides logical- communication between hosts. This distinction is subtle but important.

transport-layer protocols are implemented in the end systems but not in network routers.

Extending host-to-host delivery to process-to-process delivery is called transport-layer multiplexing and **demultiplexing**.

These two minimal transport-layer services—process-to-process data delivery and error checking—are the only two services that UDP provides! In particular, like IP, UDP is an unreliable service—it does not guarantee that data sent by one process will arrive intact (or at all!) to the destination process.