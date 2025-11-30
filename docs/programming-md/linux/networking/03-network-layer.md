# 03-Network Layer

## The Basics

- Transport layer is process-to-process communication
- Network layer is host-to-host communication

Unlike the transport and application layers, there is a piece of the network layer in each and every host and router in the network. Because of this, network-layer protocols are among the most challenging (and therefore among the most interesting!) in the protocol stack

A network-layer packet is called a **datagram**. Đừng nhầm với UDP (User Datagram Protocol) là layer 4 transport layer

- Chia Network Layer ra:
  * data plane role of each router
  * (network) Control plane role

- **Forwarding**: refers to the router-local action of transferring a packet from an input link interface to the appropriate output link interface. Forwarding is typically implemented in hardware. It is the **data-plane** functionality of the network layer.
- **Routing**: refers to the network-wide process that determines the end-to-end paths that packets take from source to destination. Routing is often implemented in software. It is a **control-plane** functionality of the network layer.

The **Routing algorithms** determine the contents of the routers’ **forwarding tables**.

## IP Datagram Format

Note that an **IP datagram** has a total of 20 bytes of header (assuming no options). If the datagram carries a TCP segment, then each datagram carries a total of 40 bytes of header (20 bytes of IP header plus 20 bytes of TCP header) along with the application-layer message.

## The Internet Protocol (IP): IPv4, Addressing, IPv6, and More

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

### Network Address Translation (NAT)

The NAT-enabled router does not look like a router to the outside world. Instead the NAT router behaves to the outside world as a single device with a single IP address.

It uses a **NAT translation table** at the NAT router, and to include port numbers as well as IP addresses in the table entries.

NAT router use **abitrarily-assigned** port numbers to map to multiple host inside its network.

### RFC 1918 & The `10.0.0.0/8` Block

**RFC 1918** is the foundational document that defines the specific blocks of private IP addresses reserved for use in internal networks (Local Area Networks or LANs).

This document was published in 1996 to address the growing exhaustion of the 4.3 billion available IPv4 public addresses.

The invention of these private ranges was a solution to allow millions of organizations and homes to use the **same IP addresses** internally without causing conflicts on the public internet.

- Non-Routable: Routers on the public internet are configured to ignore and drop any packets that have an RFC 1918 address as the destination.
- Internal Use: These addresses are designed to be used only behind a NAT (Network Address Translation) device (your router).
- Efficiency: Every device in your home or office can use a private IP address, but when that data leaves your network, the NAT device translates it to your router's single public IP address.

RFC 1918 defines three blocks of addresses, corresponding to the original Class A, B, and C networks:

---

The address space `10.0.0.0/8` is the largest of the three portions of the IP address space that is reserved in [RFC 1918] for a private network or a realm with private addresses, such as the home network.  
A realm with private addresses refers to a network whose addresses only have meaning to devices within that network.

The 10.0.0.0/8 range is meant to be used exclusively within Local Area Networks (LANs). Since this range is so large, it is typically used by big organizations, universities, or data centers that require a huge, non-conflicting block of internal addresses.

- Example: A large corporation might assign all of its U.S. offices a 10.x.x.x address block.
- Cloud Computing: Cloud platforms (like AWS and GCP) heavily use the 10.x.x.x range when customers create VPCs (Virtual Private Clouds).

IP addresses starting with 10. are blocked by routers on the public internet.

If your private IP address is 10.1.2.3, a router on the internet will not forward a packet destined for that address. This provides a basic layer of security because it means devices on the public internet cannot directly initiate contact with a device in your private network unless you specifically allow it via a router or firewall (NAT).

### IPv6 and NAT

Yes, understanding NAT (Network Address Translation) is critically important to learn IPv6, even though IPv6 was designed specifically to eliminate the need for NAT.

You need to understand the problem that NAT solved for IPv4 in order to appreciate the solution that IPv6 offers.

NAT was created as a workaround for the global shortage of IPv4 addresses.

- IPv4 Problem: Only 4.3 billion addresses. NAT lets a hundred devices share one public address (like one phone number for a whole apartment building).
- IPv6 Solution: IPv6 has a nearly infinite number of addresses. Because every single device (your phone, your laptop, your smart fridge) can have its own unique, public IP address, the complexity and overhead of NAT are removed entirely.

## The `127.0.0.0/8` Network

The entire IP range from `127.0.0.1` up to 127.255.255.254 is reserved for loopback testing and is known as the loopback network (127.0.0.0/8). All traffic sent to this range stays on your local machine and never leaves your network interface card.

When you use `localhost`, it auto resolve to both the IPv4 address (127.0.0.1) and the IPv6 address (`::1`). This ensures your application works regardless of whether the system prefers IPv4 or IPv6.  
It actually requires a DNS lookup (usually configured in the system's hosts file `/etc/hosts`).

## Subnet

A subnet (or IP network or simply a network).

To determine the subnets, detach each interface from its host or router, creating islands of isolated networks, with interfaces terminating the end points of the isolated networks. Each of these isolated networks is called a subnet

In `223.1.1.0/24`, the `/24` is the subnet mask. It indicates that the leftmost 24 bits of the 32-bit quantity define the subnet address.

The `10.0.0.x` is a `/24` network contains 256 ip addresses from `.0` to `.255`

- Một `10.0.0/24` network can be divided into two equal `/25` networks.
  - `.0` tới `.127`
  - `.127 -> .255`

Classless Inter-Domain Routing (CIDR) notation show the size of a subnet.

Convert between CIDR notation and Subnet Mask

- `10.1.1.55/28 = 255.255.255.240`

- Network ID: First IP address in each Sub-network
- Broadcast IP: Last IP address in each sub-network
- Có chức năng đặc biệt, không được assign cho user trong IP block.

- First host IP: IP address immediately after the network ID
- Last host IP: IP address immediately before the broadcast IP

## Control Plane

k

## ATM vs IP

ATM (Asynchronous Transfer Mode) và IP (Internet Protocol) là hai công nghệ mạng được phát triển với hai triết lý hoàn toàn khác nhau. Chúng từng là đối thủ cạnh tranh để trở thành nền tảng cho mạng viễn thông toàn cầu.

Câu trả lời ngắn gọn: IP đã chiến thắng. Internet hiện đại, Wi-Fi, và mạng văn phòng (Ethernet) của bạn đều chạy trên nền tảng IP.

## Routing Algorithms

Dijkstra's Algorithm là thuộc phạm vi toán học, field discrete math, graph theory.

Routing algorithm sử dụng algorithms trong math add on top mấy khái niệm không có trong toán (router, node trong toán, link cost, etc). Ví dụ Dijkstra’s least-cost path algorithm là một mathematical algorithm không liên quan gì tới router networking. Nhưng nó được dùng làm nền tảng cho Link-State routing algorithm.

Routing Algorithms with global state information are often referred to as **link-state (LS) algorithms**. The LS algorithm trình bày trong sách này là the **Dijkstra’s algorithm** trong toán học. A closely related algorithm is **Prim’s algorithm**.

The decentralized routing algorithm we’ll study is called a **distance-vector (DV) algorithm**.

Link-state

## Intra-AS Routing in the Internet: OSPF

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
