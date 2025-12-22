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

## `switch` The network device

Devices connected to a switch are able to communicate with each other via the switch. Note that they do not typically communicate with the switch itself—the switch only serves as infrastructure over which communication can occur.

The role of a switch is to connect devices within a `LAN`. For example, all of the PCs, security cameras, printers, servers, and other devices in an office are probably connected to one or more switches. For this reason, it’s common for switches to have many ports for end hosts to connect to—usually from 24 to 48 per switch.

Note that the role of a switch is not to provide connectivity between LANs or to external networks. For example, you would not connect a switch directly to the internet. For that, we need another type of device.

## Units

- The following are some common units of measuring bits:
  * 1 kilobit (kb) = 1,000 (thousand) bits
  * 1 megabit (Mb) = 1,000,000 (million) bits (1,000 kilobits)
  * 1 gigabit (Gb) = 1,000,000,000 (billion) bits (1,000 megabits)
  * 1 terabit (Tb) = 1,000,000,000,000 (trillion) bits (1,000 gigabits)

There is some confusion over whether 1 kilobit is 1,000 bits or 1,024 bits, 1 megabit is 1,000 kilobits or 1,024 kilobits, etc. The definitions listed previously are correct, and they are the terms you should know for the CCNA. The 1,024 values are a result of the binary (base-2) number system; 210 is equal to 1,024. The correct terms for the base-2 values are

- 1 kibibit (1,024 bits)
- 1 mebibit (1,024 kibibits)
- 1 gibibit (1,024 mebibits)
- 1 tebibit (1,024 gibibits)

## Ethernet

Perhaps you have heard of Ethernet before in reference to Ethernet cables. Ethernet is not one single thing but rather a collection of standards for physical wired connections as well as rules for communicating over those connections.

### Copper UTP connections

First, we will look at copper cables. This is the kind of network cable most often called an Ethernet cable, although the Ethernet standard makes use of both copper and fiber-optic cable types.

Cái jack cắm ở 2 đầu cọng dây gọi là `connector`. Cái connector này cắm vào `port` của PC, switch, router.

The `8 position 8 contact (8P8C)` connector of an ethernet cable refers to the fact that there are eight pins on the connector: one for each of the eight wires inside of the cable.  
Another name for this kind of connector is `RJ45` (RJ stands for `Registered Jack`); strictly speaking, this name is not correct, but it is commonly used when referring to Ethernet cables.

The type of cables used for these connections are called `unshielded twisted pair (UTP)` cables. There are also shielded twisted pair (STP) cables, but they are less common, so I will refer to them as UTP throughout this book. Each UTP cable contains eight individual wires inside, twisted together to make four pairs. Let’s examine the meaning of UTP:

- Unshielded—The wires do not have a metallic shield around them. This shield can reduce electromagnetic interference (EMI) but is not present in UTP cables.
- Twisted pair—The eight wires in the cable are twisted together to form four pairs of two wires each. The twisting of the wires reduces EMI between the wires of each pair.

- when connecting a PC to a switch, use a `straight-through cable`.
- When connecting two PCs, two switches, two routers, use a `crossover cable`.

### Fiber-optic connections

Copper UTP connections are still the most common type of connection within a LAN. Both the cables and the switch ports themselves are fairly inexpensive, and they are supported by nearly all modern devices that connect to a network.

UTP cable maximum 100 mét. Xa hơn thì thua.

Maximum cable length can be a problem for copper UTP connections. As you’ll see in section 3.4, increased maximum cable length is a major advantage of fiber-optic cables over copper UTP cables.

---

A typical fiber-optic connection does not use a single cable but rather **two**: one for transmitting data and one for receiving data. These cables connect to a `Small Form-Factor Pluggable (SFP)` transceiver that is inserted into an SFP port on the device. SFP transceivers are modular and must be purchased separately from the device itself.

When connecting two devices with fiber-optic cables, it’s important to connect the cables correctly: one device’s transmitter must connect to the other device’s receiver; otherwise, communication is not going to happen (similar to correctly selecting straight-through/crossover cables when connecting devices that don’t support Auto MDI-X).

---

All types of fiber-optic cabling can carry a signal farther than copper cabling, but even within the category of fiber-optic cabling, the maximum supported length can vary greatly. 

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
