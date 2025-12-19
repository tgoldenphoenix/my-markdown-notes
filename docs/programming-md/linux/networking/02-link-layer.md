# 02-The Link Layer

The packet of the link layer (the link-layer unit of data exchanged between sending and receiving adapters) is called `frame`.

For the most part, the link layer is implemented on a chip called the **network adapter**, also sometimes known as a **network interface controller (NIC)**. The network adapter implements many link layer services including framing, link access, error detection, and so on. Thus, much of a link-layer controller’s functional-ity is implemented in hardware.

## Multiple Access Links and Protocols

There are two types of network link: point-to-point links and broadcast links.

Broadcast links cần có một **Multiple-Access Protocol** to coordinate the transmissions of the active nodes. "Multiple" ở đây là một phần của cái danh từ, không phải tính từ nghĩa là "nhiều"

There are three types of Multiple-Access prototol:

1. channel partitioning protocols
2. random access protocols
3. taking-turns protocols

---

There are three types of **channel partitioning protocols**:

1. Time-division multiplexing (TDM)
2. Frequency-division multiplexing (FDM)
3. Code division multiple access (CDMA)

---

The second broad class of multiple access protocols are **random access protocols**.

## Switched Local Area Networks

A router has an IP address for each of its interfaces. For each router interface there is also an ARP module (in the router) and an adapter. Because the router in Figure 6.19 has two interfaces, it has two IP addresses, two ARP modules, and two adapters.

## MAC Address

MAC Address: Uses Hexadecimal (Base-16).

A MAC address (e.g., `00:1A:2B:3C:4D:5E`) is a `48-bit = 8 bits x 6 groups`. Each group is two hex digit representing 8 bits. We use hexadecimal (digits 0-9 and A-F) because it's a very compact and readable way to represent the underlying binary values.

MAC addresses are physical. IP addresses are logical.

An adapter’s MAC address has a flat structure (as opposed to a hierarchical structure) and doesn’t change no matter where the adapter goes.

## Terminologies

- `wifi en0`: the network interface name
  * `en` means it uses **Ethernet framing**. Wi-Fi (802.11) traffic is typically encapsulated within an Ethernet frame format for higher-level network protocols (like TCP/IP).
 
The Network Interface Card (NIC), also known as a Network Adapter, Network Card, or Network Interface Controller, is the physical hardware component that connects a computer or other device to a computer network.
