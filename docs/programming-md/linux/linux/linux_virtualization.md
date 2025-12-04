# Linux Virtualization

Broadly speaking, there are currently two approaches to virtualization:

- Hypervisors—Controls host system hardware to one degree or another, providing each guest OS the resources it needs (figure 2.3). Guest machines are run as system processes, but with virtualized access to hardware resources.
  * AWS servers, for instance, have long been built on the open source Xen hypervisor technology (although they’ve recently begun switching some of their servers to the equally open source KVM platform).
  * Other important hypervisor platforms include VMware ESXi, KVM, and Microsoft’s Hyper-V.
- Containers—Extremely lightweight virtual servers that, rather than running as full operating systems, share the underlying kernel of their host OS (see figure 2.4). Containers can be built from plain-text scripts, created and launched in seconds, and easily and reliably shared across networks.
  * The best-known container technology right now is probably Docker.
  * The Linux Container (LXC) project that we’ll be working with in this chapter was Docker’s original inspiration.

- VirtualBox (a type 2 hypervisor)
- LXC (a container manager).

## Working with VirtualBox

There’s a lot you can do with Oracle’s open source VirtualBox. You can install it on any OS (including Windows) running on any desktop or laptop computer, or use it to host VM instances of almost any major OS. 