# Networking Security

## Basic Terminologies

**Confidentiality** is the security principle that ensures data is kept private and is only accessible to authorized individuals or systems.

**Data integrity**, so that the receiving entity can check for any tampering of the datagram that may have occurred while the datagram was in transit.

## Firewalls

You have probably heard the term firewall before regarding a piece of software on your PC. For example, Windows PCs use `Microsoft Defender Firewall` by default. This kind of firewall is a `host-based firewall`. It examines network traffic entering and exiting the host device and then decides to allow or deny (block) it. It makes these decisions based on a set of defined rules.  
However, this is not the kind of firewall you need to know for the CCNA. The kind of firewall we will cover is the network firewall.

A `network firewall` is a separate hardware appliance that serves a purpose similar to a host-based firewall but on a larger scale. It inspects all traffic entering and exiting a network and decides to allow or deny it based on a set of configured rules.  
Traffic bên ngoài vào firewall > router > switch > hosts inside LAN.

## Network-Layer Security: IPsec and Virtual Private Networks

The **IP security protocol**, more commonly known as **IPsec**, provides security at the network layer. IPsec secures IP datagrams between any two network-layer entities, including hosts and routers. Many institutions (corporations, government branches, non-profit organizations, and so on) use IPsec to create virtual private networks (VPNs) that run over the public Internet.

## VPN Notes

VPN: what is it? what technology?
Pros and Cons

access control mechanisms

Vẫn dùng chung internet chứ ko phải leased line riêng.

VPN có thể là 1 hardware device (router) hoặc là 1 app

Some [quizzed](https://www.proprofs.com/quiz-school/story.php?title=vpn-practice-test)

[Virtual private network vs (actual) private network](https://www.quora.com/What-is-the-difference-between-a-VPN-and-a-private-network). VPN is a private connection over a public network (the Internet).

## VPN tunnel

[VPN tunnel](https://www.forbes.com/advisor/ca/business/software/what-is-vpn-tunnel/)

The VPN tunnel encrypts the user’s internet traffic and routes it to a remote VPN server. From there, the data is decrypted and delivered to its intended destination.

The encrypted connection enables a secure, private pathway for the user’s internet traffic. Consequently, the user’s online activities remain hidden from prying eyes and cyber threats. Also, the VPN tunnel helps to ensure all data’s confidentiality, integrity and authenticity as it travels across public networks.

### Point-to-Point Tunnelling Protocol (PPTP)

While PPTP is a popular and widely used VPN protocol, it has significant security weaknesses that make it less secure than other options. As such, it’s best to evaluate your security needs and consider alternative options when choosing this protocol.

The Point-to-Point Tunneling Protocol (PPTP) is an obsolete method for implementing virtual private networks. PPTP has many well known security issues.

pros: easy to set up, fast (for gaming & video streaming), cheap, Many VPN providers support it, Pre-installed on many devices, Compatible with most platforms
cons: has the weakest security

### L2TP/IPSec

One of the protocol’s key benefits is its broad support across multiple VPN providers and platforms, such as Windows, macOS, iOS and Android. However, it can be slower than other VPN protocols because of its resource-intensiveness and additional layers of security.

L2TP use IPSec to provide encryption

IPSec is a set of protocols for security at the network layer of the OSI model. IPSec provides authentication and encryption

The IPsec protocols AH and ESP can be implemented in a host-to-host transport mode, as well as in a network tunneling mode. 

In transport mode, only the payload of the IP packet is usually encrypted or authenticated. The routing is intact, since the IP header is neither modified nor encrypted

In tunnel mode, the entire IP packet is encrypted and authenticated. It is then encapsulated into a new IP packet with a new IP header. Tunnel mode is used to create virtual private networks for network-to-network communications (e.g. between routers to link sites), host-to-network communications (e.g. remote user access) and host-to-host communications (e.g. private chat).

## Terms

confident: tự tin | confidential: bảo mật
leased lines (kênh riêng). Link của [VNPT](https://vnpt.com.vn/doanh-nghiep/san-pham-dich-vu/kenh-thue-rieng/)
Request for Comments (RFC) [wiki](https://en.wikipedia.org/wiki/Request_for_Comments)

 **extra, intra, and inter words**
Words beginning with three Latin prefixes: Extra means "outside" or "beyond", Intra mean "within" (as in happening within a single thing)
, Inter means "between" or "among" (as in happening between two things).
[Learn more](https://www.merriam-webster.com/grammar/intra-and-inter-usage)

An **intranet** is a computer network for sharing information, easier communication, collaboration tools, operational systems, and other computing services within an organization, usually to the exclusion of access by outsiders. The term is used in contrast to public networks, such as the Internet, but uses the same technology based on the Internet protocol suite.

An intranet is sometimes contrasted to an **extranet**. While an intranet is generally restricted to employees of the organization, extranets may also be accessed by customers, suppliers, or other approved parties.