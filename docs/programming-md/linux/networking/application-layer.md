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