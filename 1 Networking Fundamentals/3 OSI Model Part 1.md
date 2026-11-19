As we recall, the purpose of networking allows 2 hosts to share data with one another, and such to send/receive data, the hosts must follow a set of rules to communicate, which is called the OSI model.

![[Screenshot 2026-09-19 at 9.25.44 AM.jpg|200]]


1. Physical Layer

Starting with the physical layer, as we know, data on computers exists in the form of bits, and something (physical) has to transport those bits between hosts.

![[Pasted image 20260919093403.png|500]]


2. Data Link Layer

The Data Link Layer is basically about communication between devices on the same local network.

A NIC (Network Interface Card) is the hardware that lets a device connect to a network. Each NIC has a MAC address, which is the Layer 2 address used to identify that interface on the local network.

When your computer wants to send data to another device on the same LAN, the data link layer wraps that data into a frame containing things like:

`source MAC address → destination MAC address → data`

A switch also operates at the Data Link Layer, it looks at the destination MAC address of a frame and decides which physical port to send it through.

However, communication often crosses multiple networks. The Data Link Layer only moves data from one MAC address to the next; routers handle the next stage.

![[Screenshot 2026-09-19 at 9.45.36 AM.jpg|500]]


3. Network Layer

The Network Layer handles communication between different networks. It uses IP addresses to identify the source and destination hosts, and routers examine those IP addresses to decide where to forward each packet next.

![[Screenshot 2026-09-19 at 9.45.05 AM.jpg|500]]



So, what's the difference between MAC addresses and IP addresses

A MAC address identifies a specific network interface on the local network, while an IP address identifies where a device is located across different networks.

- MAC address = used for the next local hop.
- IP address = used for the overall trip from source to destination.

When data crosses a router, the MAC addresses change for the next link, but the source and destination IP addresses usually stay the same across the whole route.

Let's see an example, say currently we have a packet that wants to go from `10.1.1.11` to `10.8.8.88`.

![[Screenshot 2026-09-19 at 9.48.11 AM.jpg|500]]

As we can see, it can't directly get there, it can only do so through multiple routers, so it first adds a layer 2 information from MAC address `a1a1` to `b2b2`.

![[Screenshot 2026-09-19 at 9.50.11 AM.jpg|500]]

This repeats, `b3b3` to `c4c4`, `c5c5` to `d6d6`... and so on, this is what we mean by 'hop to hop', as it's just one connection to the next, while 'end to end' means an established connection all the way.