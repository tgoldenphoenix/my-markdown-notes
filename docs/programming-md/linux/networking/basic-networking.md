# The Basics of Networking

## Terminologies

**Multiplexing** is the process of combining multiple signals or data streams into a single, shared signal or medium.  
The main purpose is to share an expensive or limited resource, like a single cable, radio frequency, or optical fiber.

---

The **network core**: the mesh of packet switches and links that interconnects the Internet’s end systems.

- In a typical home, your "Wi-Fi router" is actually a combo device:
  1. Modem: Connects to the internet provider.
  2. Router: Gets a single public IP address from the modem and manages the connection to the internet.
  3. Switch: The (usually 4) Ethernet ports on the back that let you wire multiple devices together.
  4. Access Point: Creates your Wi-Fi network.

## The Network Edge

- host = end systems. This term refers to many different things:
  - desktop computers, Linux workstations, and so-called servers that store and transmit information such as Web pages and e-mail messages
  - smartphones, tablets
  - TVs, gaming consoles, thermostats, home security systems, home appliances, watches, eye glasses, cars, traffic control systems

End systems are also referred to as **hosts** because they host (that is, run) application programs such as a Web browser program, a Web server program, an e-mail.

Hosts are sometimes further divided into two categories: **clients** and **servers**. Informally, clients tend to be desktops, laptops, smartphones, and so on, whereas servers tend to be more powerful machines that store and distribute Web pages, stream video, relay e-mail, and so on.

**access network**: the network that physically connects an end system to the first router (also known as the “edge router”).

---

Home Access: DSL, Cable, FTTH, and 5G Fixed Wireless

Today, the two most prevalent types of broadband residential access are digital subscriber line (DSL) and cable.

DSL (Digital Subscriber Line) is a technology that provides internet access by using the traditional copper telephone lines already installed in a home or building.

While DSL makes use of the telco’s existing local telephone infrastructure, cable Internet access makes use of the cable television company’s existing cable television infrastructure. A residence obtains cable Internet access from the same company that provides its cable television.

## Internet Applications

In addition to traditional applications such as e-mail and Web surfing, **Internet applications** include mobile smartphone and tablet applications, including Internet messaging, mapping with real-time road-traffic information, music streaming movie and television streaming, online social media, video conferencing, multi-person games, and location-based recommendation systems.

Internet applications run on end systems—they do not run in the packet switches in the network core.

End systems attached to the Internet provide a **socket interface** that speci-fies how a program running on one end system asks the Internet infrastructure to deliver data to a specific destination program running on another end system.  
This Internet socket interface is a set of rules that the sending program must follow so that the Internet can deliver the data to the destination program.

The postal service, of course, provides more than one service to its custom-ers. It provides express delivery, reception confirmation, ordinary use, and many more services. In a similar manner, the Internet provides multiple services to its applications. When you develop an Internet application, you too must choose one of the Internet’s services for your application.

**WebSocket** is a modern web technology built on top of that foundation.

- An internet application (or "web app") is a complete program that humans use, typically in a web browser.
- A web service (or API) is a system that machines (other programs) use to get data or perform an action.
- A web service is often a part of an internet application.

## Physical Media

Examples of physical media include twisted-pair copper wire, coaxial cable, multimode fiber-optic cable, terrestrial radio spectrum, and satellite radio spectrum.

Physical media fall into two categories: guided media and unguided media. With guided media, the waves are guided along a solid medium, such as a fiber-optic cable, a twisted-pair copper wire, or a coaxial cable. With unguided media, the waves propagate in the atmosphere and in outer space, such as in a wireless LAN or a digital satellite channel.

---

The least expensive and most commonly used guided transmission medium is twisted-pair copper wire.

Twisted pair consists of two insulated copper wires, each about 1 mm thick, arranged in a regular spiral pattern. The wires are twisted together to reduce the electrical interference from similar pairs close by. Typically, a number of pairs are bundled together in a cable by wrapping the pairs in a protective shield.

---

Like twisted pair, coaxial cable consists of two copper conductors, but the two con-ductors are concentric rather than parallel. With this construction and special insula-tion and shielding, coaxial cable can achieve high data transmission rates.

## Packet Switching

In a network application, end systems exchange **messages** with each other.

To send a message from a source end system to a destination end system, the source breaks long messages into smaller chunks of data known as **packets**. Between source and destination, each packet travels through communication links and packet switches (for which there are two predominant types, routers and link-layer switches).

### Forwarding Tables and Routing Protocols

In the Internet, every end system has an address called an IP address. When a source end system wants to send a packet to a destination end system, the source includes the destination’s IP address in the packet’s header. As with postal addresses, this address has a hierarchical structure. When a packet arrives at a router in the network, the router examines a portion of the packet’s destination address and forwards the packet to an adjacent router. More specifically, each router has a **forwarding table** that maps destination addresses (or portions of the destination addresses) to that router’s outbound links. When a packet arrives at a router, the router examines the address and searches its forwarding table, using this destination address, to find the appropriate outbound link. The router then directs the packet to this outbound link.

The Internet has a number of special **routing protocols** that are used to auto-matically set the forwarding tables. A routing protocol may, for example, determine the shortest path from each router to each destination and use the shortest path results to configure the forwarding tables in the routers.

## Circuit Switching

There are two fundamental approaches to moving data through a network of links and switches: circuit switching and packet switching

In **circuit-switched** networks, the resources needed along a path (buffers, link transmission rate) to provide for communication between the end systems are **reserved** for the duration of the communication session between the end systems.  
In **packet-switched** networks, these resources are **not reserved**; a session’s messages use the resources on demand and, as a consequence, may have to wait (that is, queue) for access to a communication link.

Traditional telephone networks are examples of circuit-switched networks. Consider what happens when one person wants to send information (voice or facsimile)  to another over a telephone network. Before the sender can send the information, the network must establish a connection between the sender and the receiver. This is a bona fide connection for which the switches on the path between the sender and receiver maintain connection state for that connection. In the jargon of telephony, this connection is called a **circuit**.

The Internet is packet-switched. If one of the links is congested because other packets need to be transmitted over the link at the same time, then the packet will have to wait in a buffer at the sending side of the transmission link and suffer a delay.  
The Internet makes its best effort to deliver packets in a timely manner, but it does not make any guarantees.

Although packet switching and circuit switching are both prevalent in today’s telecommunication networks, the trend has certainly been in the direction of packet switching.

## ISP

End systems access the Internet through **Internet Service Providers (ISPs)**.

Each ISP is in itself a network of packet switches and communication links.

The Internet is all about connecting end systems to each other, so the ISPs that provide access to end systems must also be interconnected. These lower-tier ISPs are thus interconnected through national and international upper-tier ISPs and these upper-tier ISPs are connected  directly to each other.

Each ISP network, whether upper-tier or lower-tier, is managed **independently**, runs the IP protocol (see below), and conforms to certain naming and address conventions.

The bottom level is access ISP. End systems (PCs, smartphones, Web servers, mail servers, and so on) connect into the Internet via an access ISP. The access ISP can provide either wired or wireless connectivity, using an array of access technologies including DSL, cable, FTTH, Wi-Fi, and cellular.

## Communication Links

**Coaxial cable** (cáp đồng trục): the name "coaxial" (co-axis) comes from its physical structure, where all the layers share a common center, or axis

"Copper wire" is a material, while "coaxial cable" is a specific type of cable that uses a copper wire as its core

**Terrestrial communication** uses ground-based infrastructure like cell towers for short-range signals, while **satellite communication** uses orbiting spacecraft for long-range signals, offering wider coverage but higher latency.

**Transmission Rate** of a link is measured in bits/second.

## Packet Switches

**Packets** (packages of information) là từ để chỉ chung datas đã được segmented (chia nhỏ ra) & send over the Internet through communication links & packet switches.

- A **packet switch** (Noun) takes a packet arriving on one of its incoming communication links and forwards that packet on one of its outgoing communication links.
- Packet switches có 2 loại chính:
  1. Link-layer Switches (bộ chia mạng): typically used in access networks; link-layer (layer 2) devices.
  2. Routers (bộ định tuyến): typically used in the network core; network-layer (layer 3) devices.
- Both types of switches forward packets toward their ultimate destinations.

The sequence of communication links and packet switches traversed by a packet from the send-ing end system to the receiving end system is known as a **route** or **path** through the network.

- (Link-layer) switch:
  - layer 2 (Link)
  - connects devices within a single local network (LAN)
  - Using MAC address (Physical)
- router: layer 3 (Network) connects different networks together. It uses IP Address (Logical)

While link-layer switches do not recognize IP addresses, they are capable of recognizing layer 2 addresses, such as Ethernet addresses.

---

**Packet-switched networks** (which transport packets) are in many ways similar to transportation networks of highways, roads, and intersections (which transport vehicles).

- packets are analogous to trucks
- communication links are analogous to highways and roads
- packet switches are analogous to intersections
- and end systems are analogous to buildings.

## Why connect router to modem?

Thường connect wireless (wiki) router vào modem do ISP cung cấp (thường router & modem bundle vô chung 1 physical device). Không connect switch vào modem vì sao?

Router làm 3 nhiệm vụ quan trọng mà Switch không thể làm được:

Chia sẻ 1 địa chỉ IP (NAT - Network Address Translation): Nhà cung cấp mạng (VNPT, FPT) thường chỉ cấp cho bạn một địa chỉ IP công cộng (Public IP). Router làm nhiệm vụ (gọi là NAT) để "dịch" địa chỉ này, cho phép hàng chục thiết bị trong nhà (điện thoại, laptop, TV) cùng nhau chia sẻ một kết nối Internet duy nhất đó.

Ví dụ: Router giống như một nhân viên lễ tân của tòa nhà. Mọi thư từ (dữ liệu) từ bên ngoài đều được gửi đến 1 địa chỉ của lễ tân (Router), sau đó lễ tân sẽ tự chia thư cho đúng phòng (thiết bị của bạn).

Cấp phát IP (DHCP Server):  Router tự động cấp phát các địa chỉ IP nội bộ (ví dụ: 192.168.1.2, 192.168.1.3...) cho từng thiết bị khi chúng kết nối vào mạng. Đây gọi là DHCP. Switch không làm được việc này.

Làm Cổng (Gateway) và Tường lửa (Firewall):  Router hoạt động như một "cổng ra vào" duy nhất của mạng nhà bạn. Nó ngăn chặn truy cập không mong muốn từ Internet vào các thiết bị của bạn, giúp bảo mật.

---

Switch chỉ đơn giản là một thiết bị Lớp 2 (Data Link Layer).

Nó không hiểu gì về địa chỉ IP. Nó chỉ hiểu địa chỉ MAC (địa chỉ phần cứng) của thiết bị.

Nó không thể tạo mạng, không thể cấp IP (DHCP), và không thể chia sẻ 1 kết nối Internet (NAT).

Nó giống như một ổ cắm điện: bạn cắm 1 phích vào, nó chia ra 5, 8 cổng y hệt nhau, chứ nó không quản lý dòng điện.

---

The modem's core function is **Translation (Modulate/Demodulate)**. It translates the signal from the ISP's external medium (like analog radio frequency over coaxial cable, or light over fiber) into the common digital language used in your home (Ethernet).

A modem does not perform network-layer routing. Modem serve at layer 1 & 2 of the stack.

## Delay

To get a hands-on feel for end-to-end delay in a computer network, we can make use of the `Traceroute` program.

Có một số GUI cho traceroute.

## Protocol Stack

- Mnemonic: [Armadillos Take In New Ants](https://www.instructables.com/TCPIP-Layers-Mnemonic/)
  * "In" = Internet layer (Network Layer)
  * "New Ants" = Network Access (Data Link + Physical)

- There are 5 layer from Application layer (layer 5) => Physical Layer (layer 1).
- The OSI model chia Application layer ra thêm Presentation & Session

The name of the primitive data unit depends on the layer of the protocol. At the link layer it is called a **frame**, at the IP layer a packet, and at the TCP layer a segment.

The Internet protocol stack consists of five layers: the physical, link, network, transport, and application layer. Trong sách không đề cập tới: session, presentation layers.

OSI model ở trên là gần user nhất (application layer 7), layers ở dưới là gần hardware nhất.

 There are three types of addresses: host names for the application layer, IP addresses for the network layer, and MAC addresses for the link layer.

## 05: Application Layer

The packet of information at the application layer is called a **message**. Mnemonic: HTTP messages.

This is the only layer that directly interacts with data from the user. Software applications like web browsers and email clients rely on the application layer to initiate communications. But it should be made clear that client software applications are not part of the application layer; rather the application layer is responsible for the protocols and data manipulation that the software relies on to present meaningful data to the user.

- Application layer protocols include:
  - HTTP: Web document request and transfer
  - SMTP: which provides for the transfer of e-mail messages
  - FTP: transfer of files between two end systems
  - domain name system (DNS)

### Session & Presentation Layers

The session layer

This is the layer responsible for opening and closing communication between the two devices. The time between when the communication is opened and closed is known as the session. The session layer ensures that the session stays open long enough to transfer all the data being exchanged, and then promptly closes the session in order to avoid wasting resources.

---

The presentation layer

This layer is primarily responsible for preparing data so that it can be used by the application layer; in other words, layer 6 makes the data presentable for applications to consume. The presentation layer is responsible for translation, encryption, and compression of data.

Application-layer protocols—such as HTTP and SMTP—are almost always implemented in software in the end systems; so are transport-layer protocols

## 04: Transport Layer

This layer transports application-layer messages above it between application endpoints.

In this book, we’ll refer to a transport-layer packet as a **segment**. Menomonic: transport semen.

- At this layer, commonly used protocols include:
  - Transmission Control Protocol (TCP): connection-oriented, guaranteed delivery of application-layer message; also provide flow control (that is, sender/receiver speed matching)
  - User Datagram Protocol (UDP): a lossy connectionless protocol, provides no reliability, no flow control, and no congestion control.

TCP is commonly used where all data must be intact (e.g. file share), whereas UDP is used when retaining all packets is less critical (e.g. video streaming).

TCP also breaks long messages into shorter segments and provides a congestion-control  mechanism, so that a source throttles its transmission rate when the network is con-gested.

## 03: Network Layer (IP layer)

The Internet’s network layer (IP layer) is responsible for moving network-layer packets known as **datagrams** from one host to another. Mnemonic: dùng networking (mối quan hệ) để buôn lậu vài gram ma túy kiếm chút cháo sống qua ngày.

The Internet transport-layer protocol (TCP or UDP) in a source host passes a transport-layer segment and a destination address to the network layer, just as you would give the postal service a letter with a destina-tion address

The **network layer** (IP layer) is concerned with concepts such as routing, forwarding, and addressing across a dispersed network or multiple connected networks of nodes or machines. The network layer may also manage flow control. Across the internet, the Internet Protocol v4 (IPv4) and [IPv6](https://aws.amazon.com/vpc/ipv6/) are used as the main network layer protocols.

- This layer includes:
  - IP Protocol defines the fields in the datagram as well as how the end systems and routers act on these fields.
  - (many different) Routing protocols that determine the routes that datagrams take between sources and destina-tions.

The network layer is often a mixed implementation of hardware and software.

## 02: (Data) Link Layer

In this book, we’ll refer to the link-layer packets as **frames**. Mnemonic: frame là nhỏ nhất rồi, nhỏ hơn nữa thì chỉ có bits 0s and 1s.

To move a packet from one node (host or router) to the next node in the route, the network layer relies on the services of the link layer.

The **data link** layer refers to the technologies used to connect two machines across a network where the physical layer already exists. It manages data frames, which are digital signals encapsulated into data packets. Flow control and error control of data are often key focuses of the data link layer.

- This layer includes:
  - Ethernet
  - WiFi
  - the cable access network’s DOCSIS protocol
  - PPP

## 01: Physical Layer

While the job of the link layer is to move entire frames from one network element to an adjacent network element, the job of the physical layer is to move the **individual bits** within the frame from one node to the next.

Because the physical layer and data link layers are responsible for handling commu-nication over a specific link, they are typically implemented in a network interface card (for example, Ethernet or WiFi interface cards) associated with a given link.

 The protocols in this layer are again link dependent and further depend on the actual transmission medium of the link (for example, twisted-pair copper wire, single-mode fiber optics).

 For example, Ether-net has many physical-layer protocols: one for twisted-pair copper wire, another for coaxial cable, another for fiber, and so on. In each case, a bit is moved across the link in a different way.

## TCP/IP

- TCP and IP are two separate protocols that operate on different layers of the OSI model:
  - TCP (Transmission Control Protocol) belongs to Layer 4 (Transport Layer).
  - IP (Internet Protocol) belongs to Layer 3 (Network Layer).
- They are almost always used together, so they are commonly grouped as the "TCP/IP suite."
- IP handles the addressing and routing (like the post office), while TCP handles the reliable, in-order conversation (like a phone call).

TCP/IP does not depend on any particular hardware or operating system, so devices that speak TCP/IP can all exchange data (“interoperate”) despite their many differences.

Today’s Internet is a collection of private networks owned by Inter-net service providers (ISPs) that interconnect at many so-called peering points.

TCP/IP is a protocol “suite,” a set of network protocols designed to work smoothly together. It includes several components, each defined by a standards-track RFC or series of RFCs:

- IP, the Internet Protocol, which routes data packets from one machine to
another (RFC791)
- ICMP, the Internet Control Message Protocol, which provides several
kinds of low-level support for IP, including error messages, routing assis-tance, and debugging help (RFC792)
- ARP, the Address Resolution Protocol, which translates IP addresses to
hardware addresses (RFC826)2
- UDP, the User Datagram Protocol, which provides unverified, one-way
data delivery (RFC768)
- TCP, the Transmission Control Protocol, which implements reliable, full
duplex, flow-controlled, error-corrected conversations (RFC793)

These protocols are arranged in a hierarchy or “stack”, with the higher-level proto-cols making use of the protocols beneath them.

## Network Interface Card

An Ethernet interface card is the piece of hardware that allows a computer or other device to connect to a wired network using an Ethernet cable.

It's most commonly called a Network Interface Card or NIC.

## Packet Addressing

Hardware (MAC) addressing

Internet addressing (more commonly known as IP addressing) is used. IP addresses are globally unique and hardware independent.

The mapping from IP addresses to hardware addresses is implemented at the link layer of the TCP/IP model.

hostnames (like `anhao.com`) are really just a convenient shorthand for IP addresses, and as such, they refer to network interfaces rather than computers.
