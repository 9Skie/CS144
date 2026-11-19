The story gets more complicated when  2 hosts are communicating to each other on different networks.

![[Screenshot 2026-09-19 at 2.34.21 PM.jpg|500]]

Let's set up the defaults, say host A wants to communicate with host C, which is on another network...
- Host A will know host C's IP address
- Based on host C's IP address, host A will know host C is on a foreign network


Again, we are stuck with the same problem that host A does not need host C’s MAC address. 

But they aren't on the same network right now, so we need something to communicate between networks, routers! So host A sends the packet to its default gateway, which is the router connecting its local network to other networks. 

Through the default gateway, Host A knows the router’s IP address, but again again, it doesn't know the router’s MAC address. So Host A sends an ARP request on its local network:

`Who has the default gateway's IP address?`


The router responds with its MAC address, and now host A can create the metadata for layer 3 and layer 2
- `Layer 3: IP A → IP C`
- `Layer 2: MAC A → MAC Router`

The router receives the frame, removes the Layer 2 information, and examines the destination IP address. It then determines where the packet should go next.

If Host C is directly connected to the router’s destination network, the router now needs Host C’s MAC address. The router sends its own ARP request on Host C’s local network:

`Who has IP C?`


Host C responds with its MAC address. The router then creates a new frame and sends it to Host C.
- `Layer 3: IP A → IP C`
- `Layer 2: MAC Router → MAC C`


This process repeats recursively, hop by hop. Each router only needs to figure out the next router to send the packet to, and then that router repeats the same process until the packet reaches Host C’s network... this ARP mapping can be used for any host in foreign networks.

![[Screenshot 2026-09-19 at 2.47.22 PM.jpg|500]]