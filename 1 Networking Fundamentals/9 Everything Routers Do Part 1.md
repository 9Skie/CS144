Routing is the process of moving data between networks, and a router is a device which it's purpose is to do routing.

Routers are connected to switches inside a network, and have their own IP address and MAC address, so how does it differ from a host?

The difference is their role:
- `Host = receives packets meant for itself`
`- Router = also forwards packets meant for other devices`

So the router is special because it forwards packets not destined to themselves.


Now, for routers to forward packets not destined to itself, there must be another network that exists.

But this raises a new question, didn't we say a router has 1 IP address and 1 MAC address? How would it identify itself on different networks?

So... it's more complicated than that! Routers have an IP address and a MAC address *per network that it's connected to* (called a network interface), they maintain a map of all the networks they know about through the routing table.


Let's look at this image for example:

![[Screenshot 2026-09-20 at 10.17.48 AM.jpg|500]]

R1 has two interfaces:
- interface 1: `10.0.55.1`, MAC `eee3`
- interface 2: `10.0.44.1`, MAC `eee1`

Both end in `.1` due to how the router is usually assigned as the default gateway (the first usable address) in a subnet.

The routing table then records which networks are reachable through which interface. In this example, it could be simplified to:

|Destination network|Send through|
|---|---|
|`10.0.55.0/24`|interface 1|
|`10.0.44.0/24`|interface 2|

---

These routing tables are populated through 3 different methods:
- Directly connected: added automatically for networks connected to the router’s own interfaces.
- Static routes: manually configured by an administrator.
- Dynamic routes: learned automatically from other routers using routing protocols


Let's use a new image for example:

![[Screenshot 2026-09-20 at 10.37.15 AM.jpg|500]]


In our image, we can see that there's 2 networks the router `R1` is directly connected to, and 2 network that router `R2` is connected to, these are all direct connections.

R2’s routing table:

|Type|Destination network|Interface|
|---|---|---|
|DC|`10.0.66.0/24`|interface 1|
|DC|`10.0.55.0/24`|interface 2|

R1’s routing table:

|Type|Destination network|Interface|
|---|---|---|
|DC|`10.0.55.0/24`|interface 1|
|DC|`10.0.44.0/24`|interface 2|

Here `DC` means directly connected, and the interface is relative to that router.



Now, say host C wants to send data to host A, at the L3 layer it attaches its source and destination IP address data, but `R2` drops it.

![[Screenshot 2026-09-20 at 10.49.13 AM.jpg|500]]

The reason is because when `R2` looks at it's routing table, it's not going to find an entry for the `10.0.44` network, and so it doesn't know how to reach there, so the packet is dropped.


So, we could manually add a route in `R2`'s routing table and get a static route from `10.0.44.x` to `10.0.55.1` (`R1`)`

Now when `R2` receives a packet destined for `10.0.44.x`, it knows to forward it to `R1`, which already knows how to reach that network.



But, what if we want this to be some automatic process and not have to worry about this routing ourselves? That's dynamic routing.

In this example, R1 and R2 exchange information about the networks they know how to reach. R2 knows about `10.0.66.0/24` and `10.0.55.0/24`, while R1 knows about `10.0.55.0/24` and `10.0.44.0/24`.

R2 already knows about `10.0.55.0/24`, but it learns that `10.0.44.0/24` can be reached through R1. Likewise, R1 learns that `10.0.66.0/24` can be reached through R2.

This would give us the following updated routing tables:

R2:

| Type | Destination network | Send through       |
| ---- | ------------------- | ------------------ |
| DC   | `10.0.66.0/24`      | interface 1        |
| DC   | `10.0.55.0/24`      | interface 2        |
| Dyn  | `10.0.44.0/24`      | R1 via interface 2 |

R1:

| Type | Destination network | Send through       |
| ---- | ------------------- | ------------------ |
| DC   | `10.0.55.0/24`      | interface 1        |
| DC   | `10.0.44.0/24`      | interface 2        |
| Dyn  | `10.0.66.0/24`      | R2 via interface 1 |

The exact method that routers talk to each other is called dynamic routing protocols, we won't get into the details of their exact implementations.