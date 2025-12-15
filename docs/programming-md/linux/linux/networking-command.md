# Networking Administration

## Show Network Interfaces information

`ip addr` is the modern standard for Linux, while `ipconfig` is the long-standing standard for Windows.

To see information about each network interface on your local Linux system

```
$ ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue
    state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00 brd 00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
8: eth0@if9: <BROADCAST,MULTICAST,UP,LOWER_UP>
         mtu 1500
    qdisc noqueue state UP group default qlen 1000
    link/ether 00:16:3e:ab:11:a5 brd
         ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.0.3.144/24 brd 10.0.3.255 scope
         global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::216:3eff:feab:11a5/64 scope link
       valid_lft forever preferred_lft forever
```

- `enp4s0`:
  * `en`: ethernet; `wl` for wireless LAN
  * `p4`: PCI Bus 4; Indicates the interface is connected to the PCI bus number 4.
  * `s0`: Slot 0; Indicates the interface is in the first (slot 0) position within that PCI device.

Older versions of Linux are used to assign more generic network interface names, such as `eth0` and `wlan0`. Now interfaces are named by their locations on the computer’s bus. For example, the first port on the network card seated in the third PCI bus for a Fedora system is named p3p1. The first embedded Ethernet port would be em1.

## `ping`

use `ping` to test whether your two computers can see and speak to each other.

```
$ ping 10.0.3.144
PING 10.0.3.144 (10.0.3.144) 56(84) bytes of data.
64 bytes from 10.0.3.144: icmp_seq=1
     ttl=64 time=0.063 ms
64 bytes from 10.0.3.144: icmp_seq=2 ttl=64 time=0.068 ms
64 bytes from 10.0.3.144: icmp_seq=3 ttl=64 time=0.072 ms
64 bytes from 10.0.3.144: icmp_seq=4
     ttl=64 time=0.070 ms
```

And failure will look like the following. To illustrate, I pinged an unused IP address:

```
$ ping 10.0.3.145
PING 10.0.3.145 (10.0.3.145) 56(84) bytes of data.
From 10.0.3.1 icmp_seq=1
   Destination Host Unreachable
From 10.0.3.1 icmp_seq=1 Destination Host Unreachable
```

## Checking routing Information

