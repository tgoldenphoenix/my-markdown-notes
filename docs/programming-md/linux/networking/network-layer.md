# Network Layer

unlike the transport and application layers, there is a piece of the network layer in each and every host and router in the network. Because of this, network-layer protocols are among the most challenging (and therefore among the most interesting!) in the protocol stack

A network-layer packet is called a **datagram**.

- **Forwarding**: refers to the router-local action of transferring a packet from an input link interface to the appropriate output link interface. Forwarding is typically implemented in hardware. It is the **data-plane** functionality of the network layer.
- **Routing**: refers to the network-wide process that determines the end-to-end paths that packets take from source to destination. Routing is often implemented in software. It is a **control-plane** functionality of the network layer.

The **Routing algorithms** determine the contents of the routers’ **forwarding tables**.

## The The Internet Protocol (IP): IPv4, Addressing, IPv6, and More

- Domain name must be read from left <- right.
- IP address should be read from left -> right (giống số decimal bình thường mình cũng đọc như vậy, hàng thousands -> hundreds -> tens).

The version of TCP/IP that has been in widespread use for three decades is protocol revision 4, aka IPv4. It uses four-byte IP addresses. A modernized version, IPv6, expands the IP address space to 16 bytes and incorporates several other lessons learned from the use of IPv4.

The development of IPv6 was to a large extent motivated by the concern that we are running out of 4-byte IPv4 address space.

**Dotted-Decimal notation** (`xxx.xxx.xxx.xxx`) is the format of an IPv4 address. It's a **32-bit** (4 x 8 bits) numerical label used to identify a device on a network. The format `xxx.xxx.xxx.xxx` represents four separate numbers (called "octets") separated by dots. Each xxx is a number that can range from 0 to 255 (one byte 2^8).

An address like `192.168.1.1` represent a 32-bit number (4 x 8 bits). Each of the four numbers is the decimal value of one octet (`0-255`).

An IP address is **hierarchical** because as we scan the address from left to right, we obtain more and more specific information about where the host is located in the Internet (that is, within which network, in the network of networks).

Is there a global authority that has ultimate responsibility for managing the IP address space and allocating address blocks to ISPs and other organizations?  
Indeed there is! IP addresses are managed under the authority of the **Internet Corporation for Assigned Names and Numbers** (ICANN).

---

IPv6 addresses are much longer (128 bits, 16 x 8 bits), so using decimal would be impractical. Instead, we use hexadecimal, separated by colons.

Có 8 groups separated by `:`, each group containing four hexadecimal digits. We have: `8 groups x (8 x 2) bits = 128 bits`.

## Control Plane




