# Application Layer

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

## TCP

connection-oriented service and a reliable data transfer service.

### TSL - Transport Layer Security

Neither TCP nor UDP provides any encryption - the data that the sending process passes into its socket is the same data that travels over the network to the destina-tion process.

So, for example, if the sending process sends a password in cleartext (i.e., unencrypted) into its socket, the cleartext password will travel over all the links between sender and receiver, potentially getting sniffed and discovered at any of the intervening links.

TCP can be easily enhanced at the application layer with TLS to provide security services.

## UDP

connectionless, unreliable data transfer service, no guarantee

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

The DNS protocol runs over UDP and uses port 53.

Like HTTP, FTP, and SMTP, the DNS protocol is an application-layer protocol since it relies on an underlying end-to-end transport protocol (UDP) to transfer DNS messages between communicating end systems.  
However, the role of the DNS is quite different from Web, file transfer, and e-mail applications. Unlike these applications, the DNS is not an application with which a user directly interacts.

DNS uses a large number of servers, organized in a hierarchical fashion and distributed around the world. No single DNS server has all of the mappings for all of the hosts in the Internet.

- Domain name `google.com`
- Host name:
  * `www` (for a web server)
  * `mail` (for a mail server)
  * `ftp` (for a file server)

 `www.google.com` is a host name + domain name

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

### Structure of URL

URL: Uniform Resource Locator

The `http://` part of the URl is the protocol/scheme

`http` transfer data in plain text, `https` encrypt data.

Nếu send passwords over a `http` protocol thì sẽ bị thấy.

Domain name must be read from left <- right.

`example.com.` is a **domain name**. Parts of DNs are separated by a period `.`; The trailing `.` at the end is the **root** of the Internet's namespace.

Domain name are case-insensitive.

- `example` is the **secondary level domain** (SLD); the name of the website
- `com` top level domain (TLD). It gives you an idea of what sort of an entity the organization behind the website is.

Top level domain entities could be: `.com.`, `.gov.`, `.edu.`

In `blog.example.com`, the `blog` is the **sub-domain** of the SLD `example.`  
`www` is a sub-domain. Modern websites often omit `WWW` in URLs because it’s not required for functionality, but HTTP or HTTPS is essential for website security and performance.

For example, Google’s root domain is `www.google.com`. Subdomains của Google như là: `docs.google.com`, `ads.google.com`, `keep.google.com`

Sub-domain có thể có multiple layers separated by period symbol. For example, in `https://amazon.com.scamwebsite.com`, `scamwebsite.com` is the name of the website, not `example.com`. Domain name phải đọc right -> left (như văn ngôn cổ hihi).

In `anhao.com/public/home-page` thì `/public` là **page path** or directory, `/home-page` là **resource** (HTML web page).

In `example.com/?type=public&post=new-blog-post`, the stuff appearing after the `?` symbol is called a **query string**.

Do not click links you are suspicious (email, social media, text mobile)

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
