# Network Layer

- Transport layer is process-to-process communication
- Network layer is host-to-host communication

Unlike the transport and application layers, there is a piece of the network layer in each and every host and router in the network. Because of this, network-layer protocols are among the most challenging (and therefore among the most interesting!) in the protocol stack

A network-layer packet is called a **datagram**.

- Chia Network Layer ra:
  * data plane role of each router
  * (network) Control plane role

- **Forwarding**: refers to the router-local action of transferring a packet from an input link interface to the appropriate output link interface. Forwarding is typically implemented in hardware. It is the **data-plane** functionality of the network layer.
- **Routing**: refers to the network-wide process that determines the end-to-end paths that packets take from source to destination. Routing is often implemented in software. It is a **control-plane** functionality of the network layer.

The **Routing algorithms** determine the contents of the routers’ **forwarding tables**.

## IP Datagram Format

Note that an IP datagram has a total of 20 bytes of header (assuming no options). If the datagram carries a TCP segment, then each datagram carries a total of 40 bytes of header (20 bytes of IP header plus 20 bytes of TCP header) along with the application-layer message.

## The Internet Protocol (IP): IPv4, Addressing, IPv6, and More

- Domain name must be read from left <- right.
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

## Subnet

A subnet (or IP network or simply a network).

To determine the subnets, detach each interface from its host or router, creating islands of isolated networks, with interfaces terminating the end points of the isolated networks. Each of these isolated networks is called a subnet

In `223.1.1.0/24`, the `/24` is the subnet mask. It indicates that the leftmost 24 bits of the 32-bit quantity define the subnet address.

## Control Plane

k

## ATM vs IP

ATM (Asynchronous Transfer Mode) và IP (Internet Protocol) là hai công nghệ mạng được phát triển với hai triết lý hoàn toàn khác nhau. Chúng từng là đối thủ cạnh tranh để trở thành nền tảng cho mạng viễn thông toàn cầu.

Câu trả lời ngắn gọn: IP đã chiến thắng. Internet hiện đại, Wi-Fi, và mạng văn phòng (Ethernet) của bạn đều chạy trên nền tảng IP.