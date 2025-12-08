# Linux Virtualization

Broadly speaking, there are currently two approaches to virtualization:

- `Hypervisors`—software running on a host machine that controls host system hardware to one degree or another, providing each guest OS the resources it needs. Guest machines are run as system processes, but with virtualized access to hardware resources.
  * AWS servers, for instance, have long been built on the open source Xen hypervisor technology (although they’ve recently begun switching some of their servers to the equally open source KVM platform).
  * Other important hypervisor platforms include VMware ESXi, KVM, and Microsoft’s Hyper-V.
- `Containers`—Extremely lightweight virtual servers that, rather than running as full operating systems, share the underlying kernel of their host OS. Containers can be built from plain-text scripts, created and launched in seconds, and easily and reliably shared across networks.
  * The best-known container technology right now is probably Docker.
  * The Linux Container (LXC) project that we’ll be working with in this chapter was Docker’s original inspiration.

- VirtualBox (a type 2 hypervisor)
- LXC (a container manager).

## Download ISO & Verify Checksum

Large files can sometimes become corrupted during the download process. If even a single byte within your .ISO has been changed, there’s a chance the installation won’t work. Because you don’t want to invest time and energy only to discover that there was a problem with the download, it’s always a good idea to immediately calculate the checksum (or hash) for the .ISO you’ve downloaded to confirm that everything is as it was. To do that, you’ll need to get the appropriate SHA or MD5 checksum, which is a long string looking something like this: 

`4375b73e3a1aa305a36320ffd7484682922262b3`

You should compare the appropriate string from that page with the results of a command run from the same directory as your downloaded .ISO, which might look like this: 

```
❯ sha256sum -b linuxmint-22.2-xfce-64bit.iso
dea13e523dca28e3aa48d90167a6368c63e1b3251492115417fdbf648551558f *linuxmint-22.2-xfce-64bit.iso
```

If they match, you’re in business. If they don’t (and you’ve double-checked to make sure you’re looking at the right version), then you might have to download the .ISO a second time. 

Download both `sha256sum.txt` and `sha256sum.txt.gpg`.  
Do not copy their content, use “right-click->Save Link As…” to download the files themselves and do not modify them in any way.

---

To verify the authenticity of `sha256sum.txt`, check the signature of `sha256sum.txt.gpg` by following the steps below.

Import the Linux Mint signing key:

```
❯ gpg --keyserver hkp://keys.openpgp.org:80 --recv-key 27DEB15644C6B3CF3BD7D291300F846BA25BAE09
gpg: directory '/home/anhao/.gnupg' created
gpg: keybox '/home/anhao/.gnupg/pubring.kbx' created
gpg: /home/anhao/.gnupg/trustdb.gpg: trustdb created
gpg: key 300F846BA25BAE09: public key "Linux Mint ISO Signing Key <root@linuxmint.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1
```

Check that the key was properly imported:

```
❯ gpg --list-key --with-fingerprint A25BAE09
pub   rsa4096 2016-06-07 [SC]
      27DE B156 44C6 B3CF 3BD7  D291 300F 846B A25B AE09
uid           [ unknown] Linux Mint ISO Signing Key <root@linuxmint.com>
```

Verify the authenticity of sha256sum.txt:

```
❯ gpg --verify sha256sum.txt.gpg sha256sum.txt
gpg: Signature made Tue Sep  2 09:40:21 2025 UTC
gpg:                using RSA key 27DEB15644C6B3CF3BD7D291300F846BA25BAE09
gpg: Good signature from "Linux Mint ISO Signing Key <root@linuxmint.com>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 27DE B156 44C6 B3CF 3BD7  D291 300F 846B A25B AE09
```

You need to download both file (using "save link as").

A file with the extension .txt.gpg is a plain text file (.txt) that has been encrypted using GPG (GNU Privacy Guard), a popular cryptographic software tool.

It is a file in a secure, unreadable format that requires a decryption key or passphrase to be opened and viewed.

## Working with VirtualBox

There’s a lot you can do with Oracle’s open source VirtualBox. You can install it on any OS (including Windows) running on any desktop or laptop computer, or use it to host VM instances of almost any major OS. 

You can press the host key combo to un-capture keyboard & mouse.

Host key combo: Right Ctrl

## VirtualBox Guest Additions

You can also add extra file system and device integration between VirtualBox VMs and their host through the VBox Guest Additions CD-ROM image. This provides you with features like a shared clipboard and drag and drop.

- RAM: 768 MB
- CPU

## NAT vs Bridge adapter

A Virtual Machine (VM) uses the NAT and Bridged adapter modes very differently to connect a guest operating system (the "guest machine") to the Internet.

The fundamental difference lies in where the guest machine gets its IP address and how visible it is on your local network (LAN).

NAT is the default and simplest way for a VM to get Internet access. It works by having the host machine (the one running VirtualBox or VMWare) act as a router and translator.

The Bridged Adapter mode makes the VM a full participant on your physical local network.


## Working with Linux containers (LXC) 

Create your first container: The value given to -n sets the name you’ll use for the container, and -t tells LXC to build the container with the Ubuntu template: 

`# lxc-create -n myContainer -t ubuntu`

Sticking with the Ubuntu template on an Ubuntu host is probably a safe choice. As I noted, historically, LXC has always worked best on Ubuntu hosts. Your mileage may vary when it comes to other distros. 

## Terminologies

Parallel ATA (PATA), originally AT Attachment, also known as **Integrated Drive Electronics (IDE)**, is a standard interface designed for IBM PC-compatible computers.

An `OVA file (Open Virtual Appliance`, `.ova`) is a single-file package that contains a complete, ready-to-run Virtual Machine (VM).  
It is a standardized way to distribute a virtual appliance (an operating system and pre-installed software) that can be imported directly into virtualization software like VirtualBox, VMWare, or Hyper-V.