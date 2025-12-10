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