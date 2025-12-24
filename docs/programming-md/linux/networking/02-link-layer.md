# 02-The Link Layer

## Basics & Terminologies

The packet of the link layer (the link-layer unit of data exchanged between sending and receiving adapters) is called `frame`.

For the most part, the link layer is implemented on a chip called the **network adapter**, also sometimes known as a **network interface controller (NIC)**. The network adapter implements many link layer services including framing, link access, error detection, and so on. Thus, much of a link-layer controller’s functional-ity is implemented in hardware.

The journey from one `node` to the next in the path is called a `hop`, and the job of the Data Link Layer is to provide `hop-to-hop` delivery of messages.  
A message traveling through a switch does NOT count as a hop. Chỉ có jump from host computers & router mới gọi là `hop`.

The Data Link Layer achieves this hop-to-hop delivery by using media access control (MAC) addresses, a kind of network address assigned to each port of a device. At each hop, the message is sent to the MAC address of the next hop.

The destination IP address of a message remains the same throughout the journey, whereas the destination MAC address is different at each hop. 

---

Another term for a LAN is a `Layer 2 domain`—a portion of a network where frames are switched, and hosts connected to the switch(es) can communicate with each other without the use of a router.

Physical switch có physical ports (interfaces) để cắm cables vô. Khác với cái port number cho process.

---

- `wifi en0`: the network interface name
  * `en` means it uses **Ethernet framing**. Wi-Fi (802.11) traffic is typically encapsulated within an Ethernet frame format for higher-level network protocols (like TCP/IP).
 
The Network Interface Card (NIC), also known as a Network Adapter, Network Card, or Network Interface Controller, is the physical hardware component that connects a computer or other device to a computer network.

---

`Unicast` messages can be thought of as one-to-one and `broadcast` as one-to-all. Additionally, there is another type of message called `multicast`, which is one-to-multiple (but not necessarily all).

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

## `switch` The Network device

Devices connected to a switch are able to communicate with each other via the switch. Note that they do not typically communicate with the switch itself—the switch only serves as infrastructure over which communication can occur.

The role of a switch is to connect devices within a `LAN`. For example, all of the PCs, security cameras, printers, servers, and other devices in an office are probably connected to one or more switches. For this reason, it’s common for switches to have many ports for end hosts to connect to—usually from 24 to 48 per switch.

Note that the role of a switch is not to provide connectivity between LANs or to external networks. For example, you would not connect a switch directly to the internet. For that, we need another type of device.

The role of a switch is to provide many ports for end hosts to connect to the LAN. In reality, there could be 40+ end hosts connected to each switch. 

## Units of data Transmission

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

## The Ethernet header and trailer

An Ethernet frame = E. header (14 bytes) + layer 3 diagram + E. trailer (4 bytes)

---

The `Preamble` and `Start Frame Delimiter (SFD)` are sent with each frame but are not considered part of an Ethernet frame. The reason is that they are purely a function of Layer 1, the Physical Layer. They do not contain information that influences what the receiving device decides to do with the frame (a `frame` being a layer 2 concept).

The `Preamble` and `SFD` are sent with each Ethernet frame to allow the receiving device to synchronize its `receiver clock` and prepare to receive the incoming frame. This `clock` has nothing to do with the date and time but rather with how the receiving device interprets the incoming electrical signals—the receiving device needs to determine the precise length of 1 bit. 

---

The Type/Length field (inside E. header) is a 2-byte field that can be used either to indicate the type of the encapsulated datagram (e.g., an IP version 4 packet or an IP version 6 packet) or to indicate the length of the encapsulated packet (in bytes).  
These days, in almost all cases, this field is used to indicate the type of the encapsulated packet: instead of this field indicating length, the end of the frame is indicated by a special signal after the frame. 

---

The `Frame Check Sequence (FCS)` is the only field of the Ethernet trailer. It is 4 bytes in length and is used to detect corrupted data in the frame. Before a device sends a frame, it uses an algorithm to calculate a `checksum`, a small block of data that is appended to the end of the frame as the FCS field. 

Then, when the frame’s destination host receives the frame, it calculates its own checksum for the frame (with the same algorithm) and compares it to the one calculated by the sender. If the two checksums are the same, the receiver can safely assume that the data has not been corrupted in transit. However, if the checksums calculated by the sender and receiver are different, the receiver will discard the frame—the data has been corrupted in transit (perhaps because of electromagnetic interference).

FCS is the name of the field, but the name for this kind of checksum is `cyclic redundancy check (CRC)`. The term `cyclic` refers to the kind of algorithm used to calculate the checksum. `Redundancy` means that the field is redundant—it expands the size of the message but doesn’t add any additional information. `Check` is self-explanatory—it is used to check if the frame traveled from source to destination without the data being corrupted.

## Frame switching

### MAC address learning

When a switch has to make a decision about how to forward a frame, it looks up the frame’s destination MAC address in its MAC address table, which is a list of the MAC addresses in the LAN and which port each is connected to.

But first, how does a switch build its MAC address table?  
This is the role of the Source field of the Ethernet header. When a switch receives a frame on one of its ports, it examines the Source field and creates an entry for that MAC address in its MAC address table, associating that MAC address with the port the frame was received on. This entry says “To reach this MAC address, forward the frame out of this port.”  
This makes sense: if a switch receives a frame from MAC address X on port Y, the switch knows it can reach the host with MAC address X out of port Y. This process is called `MAC address learning`.

MAC addresses learned by a switch in this manner are known as `dynamic` MAC addresses—they are automatically (dynamically) learned.  
This is in contrast to `static` MAC addresses, which are manually (statically) configured, although that is quite rare.  
A switch will remove a dynamic MAC address from its MAC address table after 5 minutes of inactivity (if it doesn’t receive a frame from that MAC address for 5 minutes); this is called `MAC aging`.

### Frame flooding and forwarding

A `frame` addressed to a single destination host is called a `unicast` frame. If the switch already has an entry for the frame’s destination MAC address in its MAC address table, it is called a `known unicast frame`.

An `unknown unicast frame` is a frame addressed to a single destination host, but the switch doesn’t have an entry for the frame’s destination MAC address in its MAC address table.

To `flood` a frame is to send it out of all ports, except the port the frame was received on. Switches take this action on receiving an `unknown unicast frame`.

- Remember what action a switch takes for each kind of unicast frame:
  * Known unicast frame (forward)—The switch will send the frame out of the port specified by the MAC address’s entry in the MAC address table.
  * Unknown unicast frame (flood)—The switch will send the frame out of all ports except the one it was received on.

---

A switch is `transparent` to its connected hosts; PC1 and PC3 address their messages directly to each other, not to SW1 or SW2, exactly as they would if they were directly connected with a single cable. This is why a message passing through a switch is not considered a hop. Also, switches do not modify the frames they switch in any way; they simply forward or flood them as appropriate. 

---

Although the MAC addresses of a switch’s ports don’t play a role when it is forwarding traffic between hosts, switches periodically exchange messages with each other and learn each other’s MAC addresses in the process.

## Address Resolution Protocol

The PCs know each other’s MAC address by using Address Resolution Protocol (ARP).

ARP allows a host to learn the MAC address of another host in the LAN.

- The ARP request message is `broadcast`.
- The ARP reply is a unicast frame sent to the MAC address of the host that sent the ARP request.

A `broadcast frame` is a frame addressed to the `broadcast MAC address: ffff.ffff.ffff`. A switch will flood broadcast frames, like unknown unicast frames. Broadcast frames are used by hosts to send messages to all other hosts in the LAN. 

If an ARP request is broadcast (addressed to all other hosts in the LAN), how does the sender specify which host’s MAC address it wants to learn? It does so by specifying the **IP address** of the host it wants to know the MAC address of.

After the ARP exchange is complete, PC1 knows PC3’s MAC address; it will store PC3’s MAC address in its `ARP table`, which is a list of IP addresses and their associated MAC addresses.

ARP can be thought of as the bridge between Layers 2 and 3 of the TCP/IP model. ARP is used to map a known Layer 3 address (IP address) to an unknown Layer 2 address (MAC address).

## Switched Local Area Networks

A router has an IP address for each of its interfaces. For each router interface there is also an `ARP module` (in the router) and an adapter. Because the router in Figure 6.19 has two interfaces, it has two IP addresses, two ARP modules, and two adapters.

## MAC Address

MAC Address: Uses Hexadecimal (Base-16).

Ethernet & Wi-Fi both use MAC address.

- A MAC address (e.g., `00:1A:2B:3C:4D:5E`):
  * `48-bit = 8 bits x 6 bytes`
  * 6 group, each group is two hex digit representing 8 bits
  * 12 hex digits

We use hexadecimal (digits 0-9 and A-F) because it's a very compact and readable way to represent the underlying binary values.

MAC addresses are physical. IP addresses are logical.

MAC addresses are not assigned by the network admin or engineer configuring the device. Instead, each port of a network device has a MAC address that is assigned to it by the manufacturer.

A MAC address is globally unique—it should not be shared by a port on any other device in the world.

An adapter’s MAC address has a flat structure (as opposed to a hierarchical structure) and doesn’t change no matter where the adapter goes.

---

To ensure that MAC addresses remain globally unique, the first half of each MAC address (the first 3 bytes) is an `organizationally unique identifier (OUI)` assigned to the manufacturer by the IEEE. Then, the manufacturer is free to use the second half to assign unique MAC addresses to each device they manufacture.

