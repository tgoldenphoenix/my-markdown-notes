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

