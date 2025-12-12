# SSH

Muốn connect SSH thì firewall phải allow SSH traffic.

- Inbound SSH connectivity
  * SSH Client (Local Machine): Initiates the connection (e.g., you type `ssh user@remote-ip`).
  * SSH Server (Remote Host): Listens passively on a specific port, waiting to accept the incoming connection.

## The importance of encryption

In the beginning, there was `Telnet` for login connections over a network at any rate. The Telnet protocol was fast and reliable and, in an innocent world made up of smaller and simpler networks, perfectly serviceable. Back then, the fact that Telnet sessions sent their data packets without encryption wasn’t a big deal.

 If you’re using Telnet to transmit private data that includes passwords and personal information in plain text over insecure networks, then you should assume it’s no longer private. In fact, anyone on the network using freely available packet-sniffing software like Wireshark can easily read everything you send and receive.

 To protect the privacy of data even if it falls into the wrong hands, security software can use what’s known as an **encryption key**, which is a small file containing a random sequence of characters.  
 The key can be applied as part of an encryption algorithm to convert plain-text, readable data into what amounts to total gibberish. At least that’s how it would appear before the key is applied through a reverse application of the same algorithm. Using the key on the encrypted version of the file converts the gibberish back to its original form.  
 As long as you and your trusted friends are the only people in possession of the key, no one else should be able to make any sense of the data, even if it’s intercepted.

 A private/public key pair to encrypt and decrypt the contents of a plain-text message.

 When you log in to a remote server, you’re doing nothing more than causing data packets containing session information to be sent back and forth between two computers. The trick of secure communications is to quickly encrypt each of those packages before it’s transmitted, and then, just as quickly, decrypt them at the other end. The SSH network protocol does this so quickly and so invisibly, in fact, that someone already used to connecting through Telnet sessions won’t see any difference.
 
## What is SSH

The SSH (Secure Shell) is a protocol originally developed by Tatu Ylonen in 1995 in response to a hacking incident in the Finnish university network.

SSH is a software package that enables secure system administration and file transfers over insecure networks.

SSH is a secure alternative to the non-protected login protocols (such as telnet, rlogin) and insecure file transfer methods (such as FTP).

The SSH protocol uses encryption to secure the connection between a client and a server. All user authentication, commands, output, and file transfers are encrypted to protect against attacks in the network.

**SSH uses TCP** to guarantee a reliable, ordered, and error-checked connection. If TCP didn't exist, SSH would have to perform its own checking and sequencing.

## How SSH work?

The SSH client drives the connection setup process and uses public key cryptography to verify the identity of the SSH server.

For two computers to be connected over SSH, each host must have SSH installed. SSH has two components: the command you use on your local machine to start a connection, and a server to accept incoming connection requests

To verify whether you have both the command and the server installed, the easiest method is to look for the relevant configuration files

```
$ file /etc/ssh/ssh_config
/etc/ssh/ssh_config: ASCII text
```

Should this return a `No such file or directory` error, then you don't have the SSH command installed.

Do a similar check for the SSH service (note the `d` in the filename):

```
$ file /etc/ssh/sshd_config
/etc/ssh/sshd_config: ASCII text
```

Install one or the other, as needed: `$ sudo dnf install openssh-clients openssh-server`

`sshd` stands for Secure Shell Daemon.

It is the server-side program that runs continuously in the background (as a daemon process) on a Linux, macOS, or Unix-like system. Its sole purpose is to enable and manage inbound SSH connections.

On nearly all Linux environments, the `sshd` server should start automatically.

On Ubuntu, you can start the ssh server by typing: `sudo systemctl start ssh` or `$ sudo systemctl enable --now sshd`

## OpenSSH

When you log in to a remote computer, your local PC is acting as a client of the remote server, so you’d use the openssh-client package. The operating system (OS) on the remote server you’re logging in to, however, is acting as a host for the shell session, so it must be running the openssh-server package. 

You can run `dpkg -s openssh-client` or `dpkg -s openssh-server` to confirm that you’ve got the right package on your machine. Because they’re built to host remote shell sessions, Linux containers will always have the full suite installed by default.

Use `systemctl status` to find out whether SSH is running on your machine.

```
$ systemctl status ssh
? ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service;
       enabled; vendor preset: enabled)
   Active: active (running) since Mon 2017-05-15 12:37:18
       UTC; 4h 47min ago
 Main PID: 280 (sshd)
    Tasks: 8
   Memory: 10.1M
      CPU: 1.322s
   CGroup: /system.slice/ssh.service
            280 /usr/sbin/sshd -D
            894 sshd: ubuntu [priv]
            903 sshd: ubuntu@pts/4
            904 -bash
           1612 bash
           1628 sudo systemctl status ssh
           1629 systemctl status ssh
[...]
```

You can force a process (like SSH) to automatically load on system startup using `systemctl enable ssh`, or to not load on startup with `systemctl disable ssh`.

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

## SSH into Remote Server Using Password

Dùng Window Git bash cũng có thể `ssh` được.

Now that you've installed and enabled SSH on the remote computer, you can try logging in with a password as a test. To access the remote computer, you must have a user account and a password.

Your remote user doesn't have to be the same as your local user. You can log in as any user on the remote machine as long as you have that user's password. For instance, I'm `sethkenlon` on my work computer, but I'm `seth` on my personal computer. If I'm on my personal computer (making it my current local machine) and I want to SSH into my work computer, I can do that by identifying myself as `sethkenlon` and using my work password.

To SSH into the remote computer, you must know its internet protocol (IP) address or its resolvable hostname. To find the remote machine's IP address, use the `ip` command (on the remote computer):

```
$ ip addr show | grep "inet "
inet 127.0.0.1/8 scope host lo
inet 10.1.1.5/27 brd 10.1.1.31 [...]
```

If the remote computer doesn't have the `ip` command, try `ifconfig` instead (or even `ipconfig` on Windows).

The address `127.0.0.1` is a special one and is, in fact, the address of `localhost`. It's a "loopback" address, which your system uses to reach itself. That's not useful when logging into a remote machine, so in this example, the remote computer's correct IP address is `10.1.1.5`. In real life, I would know that because my local network uses the `10.1.1.0` subnet. If the remote computer is on a different network, then the IP address could be nearly anything (never `127.0.0.1`, though), and some special routing is probably necessary to reach it through various firewalls. Assume your remote computer is on the same network, but if you're interested in reaching computers more remote than your own network, [read my article about opening ports in your firewall](https://opensource.com/article/20/9/firewall).

If you can ping the remote machine by its IP address or its hostname, and have a login account on it, then you can SSH into it:

```
$ ping -c1 10.1.1.5
PING 10.1.1.5 (10.1.1.5) 56(84) bytes of data.
64 bytes from 10.1.1.5: icmp_seq=1 ttl=64 time=4.66 ms
$ ping -c1 akiton.local
PING 10.1.1.5 (10.1.1.5) 56(84) bytes of data.
```

That's a success. Now use SSH to log in:

```
$ whoami
seth
$ ssh sethkenlon@10.1.1.5
bash$ whoami
sethkenlon
```

The test login works, so now you're ready to activate passwordless login (using keys).

## Using SSH Keys

`ssh remote_host`  
The `remote_host` is the IP address or domain name that you are trying to connect to. This command assumes that your username on the remote system is the same as your username on your local system.

If your username is different on the remote system, you can specify it by using this syntax: `ssh remote_username@remote_host`

Once you have connected to the server, you may be asked to verify your identity by providing a password. You can generate keys to use instead of passwords.

Type `exit` to close the SSH session and return to your local shell.

---

SSH keys can be used to automate access to servers. They are commonly used in scripts, backup systems, configuration management tools, and by developers and sysadmins. They also provide single sign-on, allowing the user to move between his/her accounts without having to type a password every time.  
However, unmanaged SSH keys can become a major risk in larger organizations.

While it is helpful to be able to log in to a remote system using passwords, it is faster and more secure to set up key-based authentication.

Functionally SSH keys resemble passwords.

- Key-based authentication works by creating a pair of keys: a private key and a public key.
  * The private key is located on the client’s machine and is secured and kept secret.
  * The public key can be given to anyone or placed on any server you wish to access.

You create a special key pair and then copy the public half of the pair to the remote host, which is the computer where you eventually want to log in.

Ideally, you should create what is called a **passphrase** and use it to authenticate yourself locally before using your key pair. Especially if you share your computer with others. If you do opt to add a passphrase, you’ll be prompted to enter it each time you use the key  
A passphrase, like a password, is a secret text string that you’ve chosen. But a passphrase will often also include spaces and consist of a sequence of real words. A password like `3Kjsi&*cn@PO` is pretty good, but a passphrase like `fully tired cares mound` might be even better because of its length and the fact that it’s relatively easy to remember. 

- **Authorized keys** are public keys that grant access. They are analogous to locks that the corresponding private key can open.
- **Identity keys** are private keys that an SSH client uses to authenticate itself when logging into an SSH server. They are analogous to physical keys that can open one or more locks.

Authorized keys and identity keys are jointly called user keys.
When you attempt to connect using a key pair, the server will use the public key to create a message for the client computer that can only be read with the private key.

The client computer then sends the appropriate response back to the server, which will tell the server that the client is legitimate.

This process is performed automatically after you configure your keys.

---

SSH public and private keys are used primarily for authentication and establishing the secure connection, but they are **not** directly used to encrypt the bulk of the data transmitted.

The actual data (the bulk transfer of text, files, and commands) is encrypted using a **symmetric cipher**, not the public/private key pair.

SSH Public and Private Keys are still called **encryption keys**, but they are used for **asymmetric encryption** (one key encrypts, the other decrypts) and their primary function in an SSH session is for authentication and key exchange, not for encrypting the bulk of the transmitted data.

### Generating a new key pair

SSH keys should be generated on the computer you wish to log in **from** (the client). This is usually your local machine. The key consists of two components: a private key, which you never share with anyone or anything, and a public one, which you copy onto any remote machine you want to have passwordless access to.

Some people create one SSH key and use it for everything from remote logins to GitLab authentication. However, I use different keys for different groups of tasks. For instance, I use one key at home to authenticate to local machines, a different key to authenticate to web servers I maintain, a separate one for Git hosts, another for Git repositories I host, and so on. In this example, I'll create a unique key to use on computers within my local area network.

`ssh-keygen -t rsa`

Your keys will be created at `~/.ssh/id_rsa.pub` and `~/.ssh/id_rsa`. `id_rsa` is the default name.

`cd ~/.ssh`

Look at the permissions of the files: `ls -l`

```
Output
-rw-r--r-- 1 demo demo  807 Sep  9 22:15 authorized_keys
-rw------- 1 demo demo 1679 Sep  9 23:13 id_rsa
-rw-r--r-- 1 demo demo  396 Sep  9 23:13 id_rsa.pub
```

As you can see, the id_rsa file is readable and writable only to the owner. This helps to keep it secret.

The `id_rsa.pub` file, however, can be shared and has permissions appropriate for this activity.

If you currently have password-based access to a server, you can **copy your public key** to it by issuing this command: `ssh-copy-id remote_host`

This will start an SSH session. After you enter your password, it will copy your public key to the server’s authorized keys file, which will allow you to log in without the password next time.

---

`$ ssh-keygen -t ed25519 -f ~/.ssh/lan`

The `-t` option stands for type and ensures that the encryption used for the key is higher than the default. The `-f` option stands for file and sets the key's file name and location. You'll be prompted to create a password for your SSH key. You should create a password for the key. This means you'll have to enter a password when using the key, but that password remains **local** and isn't transmitted across the network. After running this command, you're left with an SSH private key called `lan` and an SSH public key called `lan.pub`.

### Copying the public key over a network 

To get the public key over to your remote machine, use the `ssh-copy-id`. For this to work, you must verify that you have SSH access to the remote machine. If you can't log into the remote host with a password, you can't set up passwordless login either:

`$ ssh-copy-id -i ~/.ssh/lan.pub sethkenlon@10.1.1.5`

During this process, you'll be prompted for your login password on the remote host.

Upon success, try logging in again, but this time using the `-i` option to point the SSH command to the appropriate key (`lan`, in this example):

```
$ ssh -i ~/.ssh/lan sethkenlon@10.1.1.5
bash$ whoami
sethkenlon
```

Repeat this process for all computers on your network, and you'll be able to wander through each host without ever thinking about passwords again. In fact, once you have passwordless authentication set up, you can edit the `/etc/ssh/sshd_config` file to disallow password authentication. This prevents anyone from using SSH to authenticate to a computer unless they have your private key. To do this, open `/etc/ssh/sshd_config` in a text editor with `sudo`permissions and search for the string `PasswordAuthentication`. Change the default line to this:

`PasswordAuthentication no`

Save it and restart the SSH server (or just reboot):

```
$ sudo systemctl restart sshd && echo "OK"
OK
$
```

Once created, you can move the public key to the file `.ssh/authorized_keys` on the host computer. That way the OpenSSH software running on the host will be able to verify the authenticity of a cryptographic message created by the private key on the client. Once the message is verified, the SSH session will be allowed to begin. 

The first thing you’ll need to do is figure out which user account on the host you’ll be logging in to. In my case, it’ll be the account called `ubuntu`. The key needs to be copied to a directory called `.ssh/`, which is beneath `/home/ubuntu/`. In case it’s not there already, you should create it now using `mkdir`. 

First, though, I’ll introduce you to a cool shortcut: to run a single command, you don’t need to actually open a full SSH session on a remote host. Instead, you can append your command to the regular ssh syntax like this:

```
ubuntu@base:~$ ssh ubuntu@10.0.3.142 mkdir -p .ssh
ubuntu@10.0.3.142's password:
```

This single, multi-line command will use cat to read all the text in the `id_rsa.pub` file and store it in memory. It will then pipe that text via an SSH logon on the remote host computer. Finally, it reads the text once again, this time on the host computer, and appends it to a file called authorized_keys. If the file doesn’t yet exist, `>>` (the append tool) creates it. If a file with that name already exists, the text will be added to any content in the file. 

```
ubuntu@base:~$ cat .ssh/id_rsa.pub \
 | ssh ubuntu@10.0.3.142 \
"cat >> .ssh/authorized_keys"
ubuntu@10.0.3.142's password:
```

### Working with multiple encryption keys 

To tell OpenSSH which key you’re after, you add the -i flag, followed by the full name and location of the private key file: 

`ssh -i .ssh/mykey.pem ubuntu@10.0.3.142`

Notice the `.pem` file extension in that example? That means the key is saved with a format that’s commonly used to access all kinds of VMs, including Amazon EC2 instances.

## Safely copying files with `scp` 

Add an "s" for "secure" before `cp`.

Assuming that you knew there was already a .ssh/ directory on the remote host you worked with earlier, here’s how you could have transferred the public key (`id_rsa.pub`) to the remote host, renaming it `authorized_keys`:

```
ubuntu@base:~$ scp .ssh/id_rsa.pub \
  ubuntu@10.0.3.142:/home/ubuntu/.ssh/authorized_keys
# Overwrites the current contents of the remote authorized\_keys file
```

If there already was an `authorized_keys` file in that directory, this operation would overwrite it, destroying any existing contents. And, you can only copy or save files if the user accounts you’re using have appropriate permissions. Therefore, don’t try saving a file to, say, the /etc/ directory on a remote machine if your user doesn’t have root privileges. Before you ask, logging in to an SSH session as the root user is generally a big security no-no. 

You can, by the way, copy remote files to your local machine. This example copies a file from an AWS EC2 instance (represented by a fictitious IP address) to the specified local directory  relative to the current work directory: 

```
$ scp -i mykey.pem mylogin@54.7.61.201:/home/mylogin/backup-file.tar.gz \
  ./backups/january/

```

I should mention that there’s a third (and official) way to safely copy your key over to a remote host—the purpose-built program called ssh-copy-id: 

`$ ssh-copy-id -i .ssh/id_rsa.pub ubuntu@10.0.3.142` Automatically copies the public key to the appropriate location on the remote host.

## Using remote graphic programs over SSH connections 

The nice thing about SSH sessions is that, unburdened by layers of GUI stuff, they’re fast and efficient. But that can be a problem if the program you need to run on the remote host is of the graphic persuasion. 

Suppose you’re trying to support a user in a remote location who’s reporting trouble with a piece of desktop software like LibreOffice. If you feel that being able to launch and run the program could help diagnose and solve the problem, then it can be done using a graphic session (with the Linux X window manager) over SSH. 

## Configuring SSH

The configuration file whose settings control how remote clients will be able to log in to your machine is `/etc/ssh/sshd_config`. The /etc/ssh/ssh_config file, on the other hand, controls the way users on this machine will log in to remote hosts as a client.

When you change the configuration of SSH, you are changing the settings of the sshd server.

In Ubuntu, the main sshd configuration file is located at `/etc/ssh/sshd_config`.

Back up the current version of this file before editing: `sudo cp /etc/ssh/sshd_config{,.bak}`

`sudo vim /etc/ssh/sshd_config`

The host key declarations specify where to look for global host keys.

```
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_dsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
```

### `~/.ssh/config`

To simplify access to multiple servers, create or edit `~/.ssh/config`:

```
Host dev-server
    HostName 192.168.1.10
    User devuser
    Port 2222
    IdentityFile ~/.ssh/dev_key
```

Then connect using: `ssh dev-server`

This is useful if you manage multiple SSH keys and nonstandard ports.

## Signature algorithms

The **RSA (Rivest-Shamir-Adleman) cryptosystem** is a family of public-key cryptosystems, one of the oldest widely used for secure data transmission.

OpenSSH also supports the `ECDSA` and `ED25519` signature algorithms. You’ll find some rather obscure technical differences between the default RSA and both ECDSA and ED25519, which have the advantage of being based on elliptic curves. But all are considered reasonably secure. One thing to keep in mind with ECDSA and ED25519 is that they might not yet be fully supported with some older implementations. 

You should no longer assume that `DSA` is supported by all implementations of OpenSSH. Due to suspicions surrounding its origins, DSA is widely avoided in any case.

## Terminologies

- SSH server
- SSH client

server public key

The **Secure Shell Protocol (SSH Protocol)** is a cryptographic network protocol for operating network services securely over an unsecured network. Its most notable applications are remote login and command-line execution.

---

The company **SSH Communications Security** (<ssh.com>) was founded by Tatu Ylönen, the Finnish developer who created the original SSH protocol specification in 1995. The company now specializes in secure remote access and encrypted network monitoring solutions, but the protocol itself (SSH) is an open standard used globally.

Service: A service is software that runs in the background so it can be used by computers other than the one it's installed on. For instance, a web server hosts a web-sharing service. The term implies (but does not insist) that it's software without a graphical interface.

Host: A host is any computer. In IT, computers are called a host because technically any computer can host an application that's useful to some other computer. You might not think of your laptop as a "host," but you're likely running some service that's useful to you, your mobile, or some other computer.

- Local: The local computer is the one you or some software is using. Every computer refers to itself as localhost, for example.
- Remote: A remote computer is one you're not physically in front of nor physically using. It's a computer in a remote location.
