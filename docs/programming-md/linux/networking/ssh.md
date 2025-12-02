# SSH Notes

Muốn connect SSH thì firewall phải allow SSH traffic.

- Inbound SSH connectivity
  * SSH Client (Local Machine): Initiates the connection (e.g., you type `ssh user@remote-ip`).
  * SSH Server (Remote Host): Listens passively on a specific port, waiting to accept the incoming connection.

## What is SSH

The SSH (Secure Shell) is a protocol originally developed by Tatu Ylonen in 1995 in response to a hacking incident in the Finnish university network.

SSH is a software package that enables secure system administration and file transfers over insecure networks.

SSH is a secure alternative to the non-protected login protocols (such as telnet, rlogin) and insecure file transfer methods (such as FTP).

The SSH protocol uses encryption to secure the connection between a client and a server. All user authentication, commands, output, and file transfers are encrypted to protect against attacks in the network.

**SSH uses TCP** to guarantee a reliable, ordered, and error-checked connection. If TCP didn't exist, SSH would have to perform its own checking and sequencing.

## How SSH work?

The SSH client drives the connection setup process and uses public key cryptography to verify the identity of the SSH server.

## SSH File Transfer Protocol (SFTP)

SFTP (SSH File Transfer Protocol) is a secure file transfer protocol. It runs over the SSH protocol. It supports the full security and authentication functionality of SSH.

SFTP has pretty much replaced legacy FTP as a file transfer protocol, and is quickly replacing FTP/S. It provides all the functionality offered by these protocols, but more securely and more reliably, with easier configuration. There is basically no reason to use the legacy protocols any more.

## SSH Client

**OpenSSH** (also known as **OpenBSD Secure Shell**) is a suite of secure networking utilities based on the Secure Shell (SSH) protocol, which provides a secure channel over an unsecured network in a client–server architecture.

OpenSSH is an open-source ssh server. It is both server & client.

OpenSSH started as a fork of the free SSH program developed by Tatu Ylönen; later versions of Ylönen's SSH were proprietary software offered by SSH Communications Security. OpenSSH was first released in 1999 and is currently developed as part of the OpenBSD operating system.

OpenSSH is not a single computer program, but rather a suite of programs that serve as alternatives to unencrypted protocols like Telnet and FTP. OpenSSH is integrated into several operating systems, namely Microsoft Windows, macOS and most Linux operating systems,while the portable version is available as a package in other systems.

The `ssh` command built into macOS/Linux is is part of the OpenSSH project.

---

**PuTTY (on Windows)** is an ssh client application used to execute the protocol.

A client application translates the user's commands into the format required by the SSH Protocol and sends them across the network.

## SSH Keys

SSH keys can be used to automate access to servers. They are commonly used in scripts, backup systems, configuration management tools, and by developers and sysadmins. They also provide single sign-on, allowing the user to move between his/her accounts without having to type a password every time. This works even across organizational boundaries, and is highly convenient.

However, unmanaged SSH keys can become a major risk in larger organizations.

## SSH into Remote Server

`ssh username@your_server_ip`

Type `exit` to close the SSH session and return to your local shell.

Dùng Window Git bash cũng có thể `ssh` được.

`ssh remote_host`

The `remote_host` is the IP address or domain name that you are trying to connect to.

## Terminologies

- SSH server
- SSH client

server public key

The **Secure Shell Protocol (SSH Protocol)** is a cryptographic network protocol for operating network services securely over an unsecured network. Its most notable applications are remote login and command-line execution.

---

The company **SSH Communications Security** (<ssh.com>) was founded by Tatu Ylönen, the Finnish developer who created the original SSH protocol specification in 1995. The company now specializes in secure remote access and encrypted network monitoring solutions, but the protocol itself (SSH) is an open standard used globally.
