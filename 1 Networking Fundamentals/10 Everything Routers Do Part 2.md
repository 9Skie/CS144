Beyond routing tables that we learnt from last video, routers also have ARP tables, which map L3 addresses to L2 addresses.

![[Screenshot 2026-09-20 at 11.12.04 AM.jpg|500]]

But this ARP table isn't just for routers! Everything with an IP address has an ARP table, and the reason we need such a table is because IP addresses tell us where the packet should go at Layer 3, but the actual frame on the local network still needs a destination MAC address at Layer 2.

Let's start with an example, say host A tries to send data to host C, it constructs the L3 header, which contains the following information

- SRC: `10.0.44.9`
- DST: `10.0.66.7`

Now it's just a long series of steps:

1. Host A can tell that `10.0.66.7` is on a different network, so it cannot send the frame directly to Host C. Instead, it must send the frame to its default gateway first.

2. Host A does not know the MAC address of the default gateway (router `R1`) so it sends an ARP request for `10.0.44.1` and gets `R1`'s MAC address.

3. Host A populates it's ARP table, `R1` also populates it's ARP table

![[Screenshot 2026-09-20 at 11.21.08 AM.jpg|300]]


4. Host A now constructs the L2 header and sends it to the router, which it receives and discard the L2 header

5. `R1` now looks up the destination IP address from it's routing table, and it knows the next hop is to the router at IP address of `10.0.55.2`

6. But... `R1` doesn't know which MAC address is associated with which IP address in `10.0.55.x`, so it also sends an ARP request for the MAC address of `10.0.55.2`

7. `R2` receives the request, sends the ARP response back to `R1`, both routers populate their ARP table

8. `R1` constructs the L2 Header and sends the packet to `R2`

9. `R2` repeats this process of discarding the L2 header, looking destination IP in routing table (to know which network to send packet to), not knowing the MAC address of the IP address, sending an ARP request, getting an ARP response from host C, host C and `R2` updating their ARP table... and finally host C receiving the packet and discarding all headers


The general pattern is:

1. destination IP
2. loop through
	1. choose next hop
	2. find next-hop MAC with ARP request
	3. build L2 frame from ARP table
	4. send`

Each router repeats this until the packet reaches the destination network. The IP destination stays the same, while the MAC addresses change at every hop.

Now, this same idea works in reverse as well (for host C to send information back to host A), but because the earlier communication populated the ARP tables already, C, R2, and R1 may already know the needed MAC addresses, so they usually do not need to send ARP requests again.



Now, this same idea works in reverse as well (for host C to send information back to host A), but because the earlier communication populated the ARP tables already, C, R2, and R1 may already know the needed MAC addresses, so they usually do not need to send ARP requests again.


so the general pattern (once a connection has been established) becomes:

1. start with destination IP
2. loop through
	1. choose next hop
	2. build L2 frame from ARP table
	3. send

---

Now, we can see how this processes is repeated for any amount of routers in this path, every router just repeats the 'loop through' part:

Each Router:

1. Looks up Destination IP in Routing Table to determine Next-Hop IP
2. Adds a L2 header with Destination MAC = next Router’s MAC
    - Performs ARP as necessary

And so, the internet is really nothing more than a series of routers handing packets between other routers.

With our current mental model of the Internet, first-time communication can seem extremely expensive. If I want to connect to YouTube, it might seem like Router 1 has to ask all neighboring routers, which then ask all of their neighbors, and so on until some route to YouTube is found. That sounds like the search could spread rapidly across the network, almost like exploring every possible path instead of already knowing where to go.

That sounds almost like a Dijkstra-style search: spreading outward through the network and exploring many possible paths until the destination is reached. Wouldn’t it be much more efficient if the Internet behaved more like A*, where routers already had enough information to move traffic in the general direction of the destination instead of searching everywhere?

![[Pasted image 20260920121841.png|500]]

Just a question, no answer right now.