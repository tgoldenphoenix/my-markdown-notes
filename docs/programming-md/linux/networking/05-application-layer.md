# 05-Application Layer

## Network Application

A **socket** is the interface between the application-layer process and the transport-layer protocol within a host.

- To identify the receiving process, the sending process must know two pieces of information:
  1. the address of the destination host (IP address)
  2. an identifier that specifies the receiving process (more specifically, the receiving socket) in the destination host. This is the **port number**.

- Popular applica-tions have been assigned specific port numbers:
  * Web server: `80`
  * A mail server process (using the SMTP protocol): `25`

The Internet (and, more generally, TCP/ IP networks) makes two transport protocols available to applications, UDP and TCP. When you (as an application developer) create a new network application for the Internet, one of the first decisions you have to make is whether to use UDP or TCP.

- Mnemonic:
  * TCP => Trustworty Connecting Protocol
  * UDP => Unreliable & Đéo thể tin được Protocol

## HTTP

HTTP uses TCP as its underlying transport protocol (rather than running on top of UDP).

Default port number for HTTP is `80`

The `HEAD` HTTP method is similar to the GET method. When a server receives a request with the HEAD method, it responds with an HTTP message but it leaves out the requested object. Application developers often use the HEAD method for debug-ging.

## Electronic Mail

Simple Mail Transfer Protocol (SMTP) uses TCP

Gmail là một **email service provider**, nó sẽ có **Mail server** của riêng nó. Ngoài gmail ra còn có các ESP như: Microsoft Outlook, Yahoo mail, iCloud mail. Mỗi ESP sẽ có những mail server riêng của tụi nó.

Mail server của ESP này có thể send mail tới mail server của ESP khác.

Mình có thể tự build custom mail server nhưng sẽ có nhiều challenge.

- `@gmail.com`
- `@outlook.com`
- `@yahoo.com`
- `@icloud.com`
- Ngoài ra còn có nhiều cách config tên khác nhau

It is important to observe that SMTP does not normally use intermediate mail serv-ers for sending mail, even when the two mail servers are located at opposite ends of the world. If Alice’s server is in Hong Kong and Bob’s server is in St. Louis, the TCP connection is a direct connection between the Hong Kong and St. Louis servers.

- Client SMTP sends mail
- Server SMTP receive mail

## DNS—The Internet’s Directory Service

The DNS is (1) a **distributed database** implemented in a hierarchy of DNS servers, and (2) an **application-layer protocol** that allows hosts to query the distributed database.

The DNS protocol runs over **UDP** and uses `port 53`.

Like HTTP, FTP, and SMTP, the DNS protocol is an application-layer protocol since it relies on an underlying end-to-end transport protocol (UDP) to transfer DNS messages between communicating end systems.  
However, the role of the DNS is quite different from Web, file transfer, and e-mail applications. Unlike these applications, the DNS is not an application with which a user directly interacts.

DNS is commonly employed by other application-layer protocols, including HTTP and SMTP, to translate user-supplied hostnames to IP addresses.

DNS uses a large number of servers, organized in a hierarchical fashion and distributed around the world. No single DNS server has all of the mappings for all of the hosts in the Internet.

### The Three Classes of DNS Servers

- There are **three classes** of DNS servers organized in a hierarchy:
  1. Root DNS servers 
  2. Top-level domain (TLD) DNS servers: `com` DNS server, `org` DNS server, `edu` DNS server, etc
  3. authoritative DNS servers: `facebook.com` DNS server, `amazon.com` DNS server, `pbs.org` DNS server, `nyu.edu` DNS server, etc

- Suppose a DNS client wants to determine the IP address for the hostname `www.amazon.com`.
  1. A root DNS returns IP addresses for TLD servers (multiple of them) for the top-level domain `com`.
  2. The client then contacts one of these TLD servers, which returns the IP address of an authoritative server for `amazon.com`.
  3. Finally, the client contacts one of the authoritative servers for amazon.com, which returns the IP address for the host-name `www.amazon.com`.

There are more than 1000 **root DNS servers** instances scattered all over the world, as shown in Figure 2.18. These root servers are copies of 13 dif-ferent root servers, managed by 12 different organizations, and coordinated through the Internet Assigned Numbers Authority [IANA 2020].  
Root name servers provide the IP addresses of the TLD servers.

For each of the top-level domains—top-level domains such as `com`, `org`, `net`, `edu`, and `gov`, and all of the country top-level domains such as `uk`, `fr`, `ca`, and `jp`—there is TLD server (or server cluster).  
The company Verisign Global Registry Services maintains the TLD servers for the com top-level domain, and the company Educause maintains the TLD servers for the edu top-level domain. The network infrastructure supporting a TLD can be large and complex.  
TLD servers provide the IP addresses for authoritative DNS servers.

Every organization with publicly accessible hosts (such as Web servers and mail servers) on the Internet must provide publicly accessible DNS records that map the names of those hosts to IP addresses. An organization’s **authoritative DNS server** houses these DNS records.  
An organization can choose to implement its own authoritative DNS server to hold these records; alternatively, the organization can pay to have these records stored in an authoritative DNS server of some service provider. Most universities and large companies implement and maintain their own primary and secondary (backup) authoritative DNS server.

---

There is another important type of DNS server called the **local DNS server**. A local DNS server does not strictly belong to the hierarchy of servers but is nevertheless central to the DNS architecture.

Each ISP—such as a residential ISP or an institutional ISP—has a local DNS server (also called a default name server). When a host connects to an ISP, the ISP provides the host with the IP addresses of one or more of its local DNS servers (typically through DHCP)

A host’s local DNS server is typically “close to” the host. For an institutional ISP, the local DNS server may be on the same LAN as the host; for a residential ISP, it is typically separated from the host by no more than a few rout-ers. When a host makes a DNS query, the query is sent to the local DNS server, which acts a proxy,  forwarding the query into the DNS server hierarchy.

### Web hosting & Domain Hosting

Nếu là simple static front-end projects with HTML CSS javascript and do not require a server to run thì không cần dùng server như Apache. Just use static host like: Netlify, vercel, github pages

Purchase domain names and link to your hosting account via DNS

All live projects should use HTTPS/SSL

Full-stack projects, API cần dùng: AWS, digital ocean

Most people purchase both from the same provider

DNS match meaningful URLs with ip addresses

Có một công ty host domain, mình sẽ trả tiền mua nó.

Web hosting là chỗ store your files.

---

Terminologies

Bằng thông không giới hạn (KGH)

### Structure of Domain

- **URI (Uniform Resource Identifier)**: The broad term. It's a string that identifies a resource.
- **URL (Uniform Resource Locator)**: The specific term. It's a URI that specifies the resource's location and tells you how to get to it.

The `http://` part of the URl is the protocol/scheme.

- `http` transfer data in plain text, `https` encrypt data.
- Nếu send passwords over a `http` protocol thì sẽ bị thấy.

- Domain name must be read from left <- right.
- IP address should be read from left -> right (giống số decimal bình thường mình cũng đọc như vậy, hàng thousands -> hundreds -> tens).

- `example.com.` is a **domain name**. Parts of domain names are separated by a period `.`; 
  * The trailing `.` at the end is the **root** of the Internet's namespace.
  * `com` is the **top level domain (TLD)**. It gives you an idea of what sort of an entity the organization behind the website is.
  * `example` is the **secondary-level domain** (SLD or base domain); the name of the website

Domain name are case-insensitive.

Top level domain entities could be: `.com.`, `.gov.`, `.edu.`

- In `blog.example.com`, `blog` is the **sub-domain** of the SLD `example`.  
- `www` is also a sub-domain. Modern websites often omit `WWW` in URLs because it’s not required for functionality, but HTTP or HTTPS is essential for website security and performance.
- Google’s root domain is `www.google.com`. Subdomains của Google như là: `docs.google.com`, `ads.google.com`, `keep.google.com`, etc

Sub-domain có thể có multiple layers separated by period symbol. For example, in `https://amazon.com.scamwebsite.com`, `scamwebsite.com` is the name of the website, not `amazon.com`. Domain name phải đọc right -> left (như văn ngôn cổ hihi).

In `anhao.com/public/home-page` thì `/public` là **page path** or directory, `/home-page` là **resource** (HTML web page).

In `example.com/?type=public&post=new-blog-post`, the parts appearing after the `?` symbol is called a **query string**.

Do not click links you are suspicious (email, social media, text mobile)

- A Domain Name is the registered, high-level name of a network or organization (the "family name").
  * Defines the nameservers (NS records).
  * Example: `google.com`
- A Hostname is the specific name of a computer or server within that domain (the "person's full name").
  * Points directly to an IP Address (A or AAAA record).

- Host name example:
  * `www` (for a web server)
  * `mail` (for a mail server)
  * `ftp` (for a file server)
- sub-domain thường là hostname vì nó identify the specific server.
- `www.google.com` is a host name `www` + domain name `google`
- Thường hostname không đi một mình mà sẽ có domain name gắn đằng sau.

 In `relay1.west-coast .enterprise.com`, `relay1` is the fourth-level domain (or sub-domain). The left-most label (relay1) is typically called the **Hostname** because it identifies the specific computer (the relay server) on that network.

### Host Aliasing

A host with a complicated hostname can have one or more  alias names. For example, a hostname such as `relay1.west-coast.enterprise.com` could have, say, two aliases such as `enterprise.com` and `www.enterprise.com`. In this case, the hostname relay1 .west-coast.enterprise.com is said to be a **canonical hostname**. **Alias hostnames**, when present, are typically more mnemonic than canonical host-names. DNS can be invoked by an application to obtain the canonical hostname for a supplied alias hostname as well as the IP address of the host.

### Email Address

`bob@yahoo.com` is an email address, not a hostname.

- An email address has two parts separated by the `@` symbol:
  * `bob`: The local part (the username or mailbox). This is not a network identifier.
  * `yahoo.com`: The domain name.

To find the actual computer (host) that receives mail for this address, your computer uses DNS (Domain Name System) to look up the **MX record (Mail Exchanger record)** for the domain yahoo.com.

The MX record will point to the true hostname of the mail server (e.g., `mta-server-01.yahoo.com`), which is the computer that actually handles receiving and sorting Bob's email.

### URL Encoding

- In **URL encoding**:
  * A forward slash `/` => `%2F`
  * Colon `:` => `%3A`

Example: `http://localhost:9000/callback` => `http%3A%2F%2Flocalhost%3A9000%2Fcallback`

### Manage domains

DNS Glossary:

- **Zone File**: The DNS configuration file for a domain.
- **Host Record**: Specifies the subdomain (e.g., @ = root, www, mail).

**DNS (Domain Name System) records** are instructions stored in your domain’s zone file. They translate human-readable domain names (like yourdomain.com) into technical data (like IP addresses or mail server locations) that browsers and email clients use.

Common DNS Record Types and What They Do:

- Type `A` points domain to an IPv4 address. Example: `@ → 192.0.2.1`
- Type `AAAA` points to an IPv6 address; Example: `@ → 2001:db8::1`
- `CNAME` Creates an alias to another domain; `www → yourdomain.com`

In a DNS zone file, the `@` symbol is a shortcut representing the domain name that the zone file is authoritative for, often called the "current origin". When you see an "@" in a DNS record, it signifies that the record applies to the root of the domain itself, rather than a specific subdomain like `www.` or "mail"

## Peer-to-Peer File Distribution
