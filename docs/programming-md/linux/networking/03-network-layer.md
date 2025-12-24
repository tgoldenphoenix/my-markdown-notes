# 03-Network Layer

Or Internet Layer cũng đúng

## The Basics & Terminologies

- Transport layer is responsible for **process-to-process communication**.
- Network layer is responsible for **host-to-host communication**.

Unlike the transport and application layers, there is a piece of the network layer in each and every host and router in the network. Because of this, network-layer protocols are among the most challenging (and therefore among the most interesting!) in the protocol stack.

A network-layer packet is called a `datagram`. Đừng nhầm với UDP (User Datagram Protocol) là một layer 4 (transport layer) protocol.  
Đôi khi nó cũng được gọi là `packet`.

- Chia Network Layer ra:
  - data plane role of each router
  - (network) Control plane role

- **Forwarding**: refers to the router-local action of transferring a packet from an input link interface to the appropriate output link interface.
  - Forwarding is typically implemented in hardware. It is the **data-plane** functionality of the network layer.
- **Routing**: refers to the network-wide process that determines the end-to-end paths that packets take from source to destination.
  - Routing is often implemented in software.
  - It is a **control-plane** functionality of the network layer.

The **Routing algorithms** determine the contents of the routers’ **forwarding tables**.

## IP Datagram Format

Note that an **IP datagram** has a total of 20 bytes of header (assuming no options). If the datagram carries a TCP segment, then each datagram carries a total of 40 bytes of header (20 bytes of IP header plus 20 bytes of TCP header) along with the application-layer message.

### The IPv4 header

Each row is 4 bytes (32 bits).

The Options field is optional (and variable in size), so the length of the IPv4 header is variable. Without the Options field, the header is `20 bytes` in length, from the first bit of the Version field to the last bit of the Destination Address field. With the Options field at its maximum size (40 bytes), the IPv4 header is 60 bytes in length. However, the Options field is rarely used and is beyond the scope of the CCNA exam.

---

The next two fields are `Differentiated Services Code Point (DSCP)`, which is 6 bits in length, and `Explicit Congestion Notification (ECN)`, which is 2 bits in length. This byte of the IPv4 header used to be called the `Type of Service` field and still is sometimes, but DSCP + ECN is the current definition. 

These fields are used for `Quality of Service (QoS)`, which is a network feature used to prioritize specific types of network traffic over other types.

---

The `Identification`, `Flags`, and `Fragment Offset` fields, 32 bits in total, are used together to support packet `fragmentation`—when a datagram is broken up into multiple smaller datagrams called `fragments`. IPv4 uses a concept called `maximum transmission unit (MTU)` to indicate the maximum size a packet should be, and any packet larger than the MTU will be fragmented. Then, the final destination host of the packet reassembles the fragments to restore the original packet.

---

The Time To Live (TTL) field is an 8-bit field. When a host sends a packet, it will set an initial value in this field (a common value is 64), and then each router that forwards the packet will decrease the value in this field by 1. If the value reaches 0, the router will drop the packet.

Loops should not occur in a properly configured network, but mistakes can happen. The TTL field prevents packets from looping indefinitely; once the packet’s TTL reaches 0, it will be dropped.

## What’s Inside a Router?

Routers are not used to connect many end hosts within a LAN. Instead, they are placed at the edge of a LAN and used to enable communications between LANs and external networks, such as the internet.

You might be wondering, “If that’s a router, what is the wireless router that connects my home network to the internet?” A **wireless router** (also known as a Wi-Fi router or home router) is not just a router; it’s a multifunctional network device that combines the roles of multiple different network devices.  
These devices typically fill the roles of a router, switch, wireless access point (to provide Wi-Fi connectivity), and firewall all in one device. They are perfect for a small office/home office (SOHO) network with only a few users. However, in enterprise networks, it’s simply not feasible for a single device to fulfill all necessary roles. 

## IPv4

- Domain name must be read from right -> left.
- IP address should be read from left -> right (giống số decimal bình thường mình cũng đọc như vậy, hàng thousands -> hundreds -> tens).

The version of TCP/IP that has been in widespread use for three decades is protocol revision 4, aka IPv4. It uses four-byte IP addresses. A modernized version, IPv6, expands the IP address space to 16 bytes and incorporates several other lessons learned from the use of IPv4.

The development of IPv6 was to a large extent motivated by the concern that we are running out of 4-byte IPv4 address space.

**Dotted-Decimal notation** (`xxx.xxx.xxx.xxx`). It's a `32-bit = 4 groups x 8 bits`. Each group is a decimal number that can range from `0-255` (corresponding to one byte, 8 bits, 2 hex digit).

Example: `192.168.1.10`

An IP address is **hierarchical** because as we scan the address from left to right, we obtain more and more specific information about where the host is located in the Internet (that is, within which network, in the network of networks).

Is there a global authority that has ultimate responsibility for managing the IP address space and allocating address blocks to ISPs and other organizations?  
Indeed there is! IP addresses are managed under the authority of the **Internet Corporation for Assigned Names and Numbers** (ICANN).

A portion of an interface’s IP address will be determined by the subnet to which it is connected.

Một host machine như máy laptop cá nhân sẽ có một **host interface** tương ứng với một globally unique IP address. Một router có 3 **router interfaces** thì mỗi interface sẽ có một IP address của riêng nó.

---

`XXXX:XXXX:XXXX:XXXX:XXXX:XXXX:XXXX:XXXX`

IPv6 addresses are much longer, `128 bits = 8 groups x 16 bits`. Each group of four hexadecimal digits (e.g., XXXX) represents 16 bits. Using decimal would be impractical. Instead, we use hexadecimal, separated by colons.

Example: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`

host PC have ip address, router interfaces have ip addresses. Switches do not have ip address.

## Subnet

A subnet (or IP network or simply a network).

To determine the subnets, detach each interface from its host or router, creating islands of isolated networks, with interfaces terminating the end points of the isolated networks. Each of these isolated networks is called a subnet

In `223.1.1.0/24`, the `/24` is the subnet mask. It indicates that the leftmost 24 bits of the 32-bit quantity define the subnet address.

The `10.0.0.x` is a `/24` network contains 256 ip addresses from `.0` to `.255`

- Một `10.0.0/24` network can be divided into two equal `/25` networks.
  * `.0` tới `.127`
  * `.127 -> .255`

Classless Inter-Domain Routing (CIDR) notation show the size of a subnet.

Convert between CIDR notation and Subnet Mask

- `10.1.1.55/28 = 255.255.255.240`

- Network ID: First IP address in each Sub-network
- Broadcast IP: Last IP address in each sub-network
- Có chức năng đặc biệt, không được assign cho user trong IP block.

- First host IP: IP address immediately after the network ID
- Last host IP: IP address immediately before the broadcast IP

The network portion of an IPv4 address is often called the `prefix` or `network prefix`. All hosts in the same LAN will share the same network portion.

---

Instead of indicating the prefix length with /X, another common method is to use a `netmask` (subnet mask)—another string of 32 bits that is paired with an IP address to indicate which bits of the IP address are the network portion and which are the host portion.

A bit in the netmask that is set to 1 means the bit in the same position of the IP address is part of the network portion; a bit in the netmask that is set to 0 means the bit in the same position of the IP address is part of the host portion. 

Like IPv4 addresses, netmasks are usually written in dotted decimal notation. 

For example, an IPv4 address (`172.16.20.21`) with a netmask (`255.255.0.0`). The first 16 bits of the netmask are 1, meaning the first 16 bits of the IPv4 address are the network portion. This is equivalent to `172.16.20.21/16`

- Prefix length: `/8` = netmask: `255.0.0.0`
- Prefix length: `/16` = netmask: `255.255.0.0`
- Prefix length: `/24` = netmask: `255.255.255.0`

A netmask is always a series of 1s followed by a series of 0s; this is because IPv4 addresses are always structured to have the network portion on the left (the most significant bits) and the host portion on the right (the least significant bits). Netmasks like `0.0.0.255` or `255.0.255.0` are not possible.

## Attributes of an IPv4 network

The `network address` is the first address of any network, and it is used to identify the network; it cannot be assigned to a host. An IPv4 address is a network address if all bits of its host portion are set to 0.

`192.168.100.0` is a network address, as indicated by the host portion of 00000000. This address is used to identify the `192.168.100.0/24` network as a whole and cannot be assigned to a host. `192.168.100.100` is a host address in the 192.168.100.0/24 network.

---

The `broadcast address` is the last address of any network, and like the network address, it can’t be assigned to a host. The broadcast address can be used to address a message to all hosts in the local network. An IPv4 address is a broadcast address if all bits of its host portion are set to 1

`192.168.100.255` is a broadcast address, as indicated by the host portion of 11111111. This address can be used to address a message to all hosts in the 192.168.100.0/24 network.

To send a message to all devices on the local network, hosts will usually address messages to `255.255.255.255`, rather than the broadcast address of their local network. `255.255.255.255` is a specially reserved broadcast address. However, the broadcast address 192.168.100.255 can be used by hosts in other networks to send a message to all hosts in the 192.168.100.0/24 network.

---

The **maximum number of hosts** in a network is the number of IP addresses available to assign to hosts connected to the network. To calculate the total number of IP addresses in a network, the formula is $2^y$, where y is the number of host bits.

For example, with a `/24` prefix length, there are eight host bits; $2^8=256$, so there are 256 total IP addresses in a /24 network (such as `192.168.100.0/24`).

However, because the network and broadcast addresses of each network can’t be assigned to hosts, we have to **subtract 2** from the total number of addresses in the network to find the maximum number of hosts. Therefore, the formula to determine the maximum number of hosts in a network is actually $2^y − 2$. For example, the maximum number of hosts of a /24 network is 254 ($2^8 − 2$). The following are the maximum number of hosts in networks with /8, /16, and /24 prefix lengths:

- `/8`: $2^{24} – 2$ = 16,777,214 hosts
- `/16`: $2^{16} – 2$ = 65,534 hosts
- `/24`: $2^8 – 2$ = 254 hosts

---

First and last usable addresses

The `first usable address` of a network is the first IP address that can be assigned to a host; in other words, it’s the first IP address after the network address. It is simple to calculate—just add one to the network address (change the least significant bit to 1).

`192.168.100.1` is the first usable address of the 192.168.100.0/24 network. It is the first address after the network address.

The first usable address of a network is often assigned to that network’s router. 

The `last usable address` of a network is the last IP address that can be assigned to a host; it’s the last IP address before the broadcast address. This address is also simple to find—subtract 1 from the broadcast address (change the least significant bit to 0). 

192.168.100.254 is the last usable address of the 192.168.100.0/24 network. It is the last address before the broadcast address.

If you know the first and last usable addresses, you know the `range of usable addresses`: from the first usable address to the last usable address. For example, the range of usable addresses in the 192.168.100.0/24 network is from 192.168.100.1 to 192.168.100.254: 254 addresses in total.

This process will become more challenging when we cover subnetting in chapter 11 of this book. When subnetting, we use prefix lengths that do not fit neatly between octets of an IP address, such as /19, /23, /28, etc. In that case, it is important to be proficient at converting between decimal and binary so you can identify the network and host bits, convert the host bits to 0 or 1 as necessary, convert them back to decimal, etc.

## IPv4 address classes

k

## Configuring IPv4 addresses on a router

End hosts like PCs usually receive their IP addresses automatically using Dynamic Host Configuration Protocol (DHCP). However, the IP addresses of network infrastructure devices like routers are usually manually configured.

## Network Address Translation (NAT)

The NAT-enabled router does not look like a router to the outside world. Instead the NAT router behaves to the outside world as a single device with a single IP address.

It uses a **NAT translation table** at the NAT router, and to include port numbers as well as IP addresses in the table entries.

NAT router use **abitrarily-assigned** port numbers to map to multiple host inside its network.

## IPv6 and NAT

Yes, understanding NAT (Network Address Translation) is critically important to learn IPv6, even though IPv6 was designed specifically to eliminate the need for NAT.

You need to understand the problem that NAT solved for IPv4 in order to appreciate the solution that IPv6 offers.

NAT was created as a workaround for the global shortage of IPv4 addresses.

- IPv4 Problem: Only 4.3 billion addresses. NAT lets a hundred devices share one public address (like one phone number for a whole apartment building).
- IPv6 Solution: IPv6 has a nearly infinite number of addresses. Because every single device (your phone, your laptop, your smart fridge) can have its own unique, public IP address, the complexity and overhead of NAT are removed entirely.

## Special IP addresses

There is a comprehensive list of special and reserved IPv4 addresses defined by standards organizations like IANA and various RFCs. These addresses are not used for public internet routing but are reserved for specific local, diagnostic, or administrative purposes.

### Private and Internal Use: RFC 1918

**RFC 1918** is the foundational document that defines the specific blocks of private IP addresses reserved for use in internal networks (Local Area Networks or LANs).

These addresses are non-routable on the public internet and are used exclusively within local, private networks (LANs) behind a NAT device (router)

This document was published in 1996 to address the growing exhaustion of the 4.3 billion available IPv4 public addresses.

The invention of these private ranges was a solution to allow millions of organizations and homes to use the **same IP addresses** internally without causing conflicts on the public internet.

- Non-Routable: Routers on the public internet are configured to ignore and drop any packets that have an RFC 1918 address as the destination.
- Internal Use: These addresses are designed to be used only behind a NAT (Network Address Translation) device (your router).
- Efficiency: Every device in your home or office can use a private IP address, but when that data leaves your network, the NAT device translates it to your router's single public IP address.

RFC 1918 defines three blocks of addresses, corresponding to the original Class A, B, and C networks:

---

- **Class A:**
  - CIDR notation: `10.0.0.0/8`
  - Subnet mask: `255.0.0.0`
  - Total Host IPs: 16.7 Million
  - Primary Purpose: Large corporate networks, Cloud VPCs.
- **Class B:**
  - CIDR notation: `172.16.0.0/12`
  - Subnet mask: `255.240.0.0`
  - Total Host IPs: 1.04 Million
  - Primary Purpose: Medium-sized organizations.
- **Class C :**
  - CIDR notation: `192.168.0.0/16`
  - Subnet mask: `255.255.0.0`
  - Total Host IPs: 65,536
  - Primary Purpose: Home and small office networks.

---

The address space `10.0.0.0/8` is the largest of the three portions of the IP address space that is reserved in [RFC 1918] for a private network or a realm with private addresses, such as the home network.  
A realm with private addresses refers to a network whose addresses only have meaning to devices within that network.

The 10.0.0.0/8 range is meant to be used exclusively within Local Area Networks (LANs). Since this range is so large, it is typically used by big organizations, universities, or data centers that require a huge, non-conflicting block of internal addresses.

- Example: A large corporation might assign all of its U.S. offices a 10.x.x.x address block.
- Cloud Computing: Cloud platforms (like AWS and GCP) heavily use the 10.x.x.x range when customers create VPCs (Virtual Private Clouds).

IP addresses starting with `10.` are blocked by routers on the public internet.

If your private IP address is `10.1.2.3`, a router on the internet will not forward a packet destined for that address. This provides a basic layer of security because it means devices on the public internet cannot directly initiate contact with a device in your private network unless you specifically allow it via a router or firewall (NAT).

### Diagnostics and Loopback

**The Loopback Network** `127.0.0.0/8` is reserved for internal self-testing. Any data sent to this range immediately loops back to the local device. 127.0.0.1 is the most common address.

The entire IP range from `127.0.0.1` up to `127.255.255.254` is reserved for loopback testing and is known as the loopback network (127.0.0.0/8). All traffic sent to this range stays on your local machine and never leaves your network interface card.

When you use `localhost`, it auto resolve to both the IPv4 address (127.0.0.1) and the IPv6 address (`::1`). This ensures your application works regardless of whether the system prefers IPv4 or IPv6.  
It actually requires a DNS lookup (usually configured in the system's hosts file `/etc/hosts`).

---

**The Current Network** `0.0.0.0/8` is ssed by devices to refer to their own network or to signal "any network" (e.g., 0.0.0.0 as the source address).

### Broadcast and Unspecified

**The Limited Broadcast** `255.255.255.255` is used to send a packet to every device on the local network (LAN segment), regardless of its specific network address.

---

**The Default Route** `0.0.0.0/0` is the "catch-all" route in a routing table. If a router doesn't know where to send a packet, it sends it to the destination specified by this address (the default gateway).

It represents the largest, most general network possible: **all IPv4 addresses** (the entire 32-bit address space).

- The primary function of the default route is to act as the last-resort instruction for a router.
  1. Specific Routes: A router’s first job is always to check for the most specific (longest) match in its routing table. For example, a route to `192.168.1.0/24` is more specific than the default route.
  2. The Default Route: If the router receives a packet destined for an IP address that is not listed in any specific route (like a server on the public internet), it uses the `0.0.0.0/0` entry.

This ensures that the router never drops a packet just because it doesn't know the exact path. It sends the unknown packet to the device listed as the next hop for the default route (usually the ISP's router), trusting that the next device knows how to handle it.

Routers always prefer the route with the longest (most specific) prefix. Since 0.0.0.0/0 is the shortest, it is only considered if no other route matches the destination IP.

---

In AWS's security group, a `CidrIp: '0.0.0.0/0` inside an ingress rule allows traffic from any source IP address.

In this scenario, the cidrIp define the source network. In the routing table, however, it defines the destination network if the router doesn't know any better.

`0.0.0.0/0` is the range from `0.0.0.0 -> 255.255.255.255` (a range that contains every possible IP address.) Trong AWS, nếu gặp `0.0.0.0/0` thì là all (inbound & outbound).

## Ping

Ping is a component of the Internet Control Message Protocol (ICMP)

Like ARP, ping consists of two messages: an `ICMP echo request` and an `ICMP echo reply`. However, unlike ARP, both messages used by ping are unicast.

Ping can also be used to measure the `round-trip time (RTT)` between two hosts—the time it takes a message to travel from one host to another and back.

## ATM vs IP

`ATM (Asynchronous Transfer Mode)` và IP (Internet Protocol) là hai công nghệ mạng được phát triển với hai triết lý hoàn toàn khác nhau. Chúng từng là đối thủ cạnh tranh để trở thành nền tảng cho mạng viễn thông toàn cầu.

Câu trả lời ngắn gọn: IP đã chiến thắng. Internet hiện đại, Wi-Fi, và mạng văn phòng (Ethernet) của bạn đều chạy trên nền tảng IP.

## Routing Algorithms

Dijkstra's Algorithm là thuộc phạm vi toán học, field discrete math, graph theory.

Routing algorithm sử dụng algorithms trong math add on top mấy khái niệm không có trong toán (router, node trong toán, link cost, etc). Ví dụ Dijkstra’s least-cost path algorithm là một mathematical algorithm không liên quan gì tới router networking. Nhưng nó được dùng làm nền tảng cho Link-State routing algorithm.

Routing Algorithms with global state information are often referred to as **link-state (LS) algorithms**. The LS algorithm trình bày trong sách này là the **Dijkstra’s algorithm** trong toán học. A closely related algorithm is **Prim’s algorithm**.

The decentralized routing algorithm we’ll study is called a **distance-vector (DV) algorithm**.

Link-state

## Intra-AS Routing in the Internet: OSPF

OSPF is a routing protocol that operates within a single ISP’s network.

Routers are organized into **Autonomous System** (AS). An autonomous system is identified by its globally unique **autonomous system number** (ASN).

**Intra-AS routing** (also called Interior Gateway Routing) refers to the routing of data packets inside a single Autonomous System (AS).

**Open Shortest Path First** (OSPF) routing and its closely related cousin, IS-IS, are widely used for intra-AS routing in the Internet.

## Routing Among the ISPs: BGP

We just learned that OSPF is an example of an intra-AS routing protocol. When routing a packet between a source and destination within the same AS, the route the packet follows is entirely determined by the intra-AS routing protocol. However, to route a packet across multiple ASs, say from a smartphone in Timbuktu to a server in a datacenter in Silicon Valley, we need an **inter-autonomous system routing protocol**. Since an inter-AS routing protocol involves coordination among multiple ASs, communicating ASs must run the same inter-AS routing protocol. In fact, in the Internet, all ASs run the same inter-AS routing protocol, called the **Border Gateway Protocol**, more commonly known as **BGP**.

BGP is arguably the most important of all the Internet protocols (the only other contender would be the **IP protocol**), as it is the protocol that glues the thousands of ISPs in the Internet together. As we will soon see, BGP is a decentralized and asynchronous protocol in the vein of distance-vector routing.

For destinations that are within the same AS, the entries in the router’s forwarding table are determined by the AS’s intra-AS routing protocol. But what about destinations that are outside of the AS? This is precisely where BGP comes to the rescue.

CIDR stands for **Classless Inter-Domain Routing**. A CIDR address looks like a standard IP address followed by a slash and a number: `192.168.1.0/24`.

In BGP, packets are not routed to a specific destination address, but instead to CIDRized prefixes, with each prefix representing a subnet or a collection of subnets.

For each AS, each router is either a gateway router or an internal router. A **gateway router** is a router on the edge of an AS that directly connects to one or more routers in other ASs. An internal router connects only to hosts and routers within its own AS.
