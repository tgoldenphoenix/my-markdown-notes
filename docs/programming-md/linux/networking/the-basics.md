# The Basics of Networking

## Terminologies

- host = end systems:
  * desktop computers, Linux workstations, and so-called servers that store and transmit information such as Web pages and e-mail messages
  * smartphones, tablets
  * TVs, gaming consoles, thermostats, home security systems, home appliances, watches, eye glasses, cars, traffic control systems

End systems are also referred to as **hosts** because they host (that is, run) appli-cation programs such as a Web browser program, a Web server program, an e-mail

Hosts are sometimes further divided into two categories: **clients** and **servers**. Infor-mally, clients tend to be desktops, laptops, smartphones, and so on, whereas servers tend to be more powerful machines that store and distribute Web pages, stream video, relay e-mail, and so on.

- **Packet switch** (Noun.) có 2 loại chính:
  * routers
  * link-layer switches

---

End systems access the Internet through **Internet Service Providers (ISPs)**.

Each ISP is in itself a network of packet switches and communication links.

The Internet is all about connecting end systems to each other, so the ISPs that provide access to end systems must also be interconnected. These lower-tier ISPs are thus interconnected through national and international upper-tier ISPs and these upper-tier ISPs are connected  directly to each other.

Each ISP network, whether upper-tier or lower-tier, is managed **independently**, runs the IP protocol (see below), and conforms to certain naming and address conventions.

---

In addition to traditional applications such as e-mail and Web surfing, **Internet applications** include mobile smartphone and tablet applications, including Internet messaging, mapping with real-time road-traffic information, music streaming movie and television streaming, online social media, video conferencing, multi-person games, and location-based recommendation systems.

**access network**: the network that physically connects an end system to the first router.

**Multiplexing** is the process of combining multiple signals or data streams into a single, shared signal or medium.  
The main purpose is to share an expensive or limited resource, like a single cable, radio frequency, or optical fiber.

---

coaxial cable: cáp đồng trục

**Terrestrial communication** uses ground-based infrastructure like cell towers for short-range signals, while **satellite communication** uses orbiting spacecraft for long-range signals, offering wider coverage but higher latency.

The **network core**: the mesh of packet switches and links that interconnects the Internet’s end systems.

