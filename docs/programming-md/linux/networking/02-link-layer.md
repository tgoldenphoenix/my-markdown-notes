# The Link Layer

Packet of the link layer is called **frame**.

For the most part, the link layer is implemented on a chip called the **network adapter**, also sometimes known as a network interface controller (NIC). The network adapter implements many link layer services including framing, link access, error detection, and so on. Thus, much of a link-layer controller’s functional-ity is implemented in hardware.

## MAC Address

MAC Address: Uses Hexadecimal (Base-16).

A MAC address (e.g., `00:1A:2B:3C:4D:5E`) is a `48-bit = 8 bits x 6 groups`. Each group is two hex digit representing 8 bits. We use hexadecimal (digits 0-9 and A-F) because it's a very compact and readable way to represent the underlying binary values.

MAC addresses are physical. IP addresses are logical.

An adapter’s MAC address has a flat structure (as opposed to a hierarchical structure) and doesn’t change no matter where the adapter goes.
