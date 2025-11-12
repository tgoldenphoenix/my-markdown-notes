# Network Layer

unlike the transport and application layers, there is a piece of the network layer in each and every host and router in the network. Because of this, network-layer protocols are among the most challenging (and therefore among the most interesting!) in the protocol stack

A network-layer packet is called a **datagram**.

- **Forwarding**: refers to the router-local action of transferring a packet from an input link interface to the appropriate output link interface. Forwarding is typically implemented in hardware. It is the **data-plane** functionality of the network layer.
- **Routing**: refers to the network-wide process that determines the end-to-end paths that packets take from source to destination. Routing is often implemented in software. It is a **control-plane** functionality of the network layer.

The **Routing algorithms** determine the contents of the routers’ **forwarding tables**.

## What’s Inside a Router?

