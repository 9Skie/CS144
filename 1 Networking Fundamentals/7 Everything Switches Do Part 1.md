Switching is the process of moving data within networks, while switches are the devices that perform switching, they only use the layer 2 header of MAC addresses to make decisions.

![[Screenshot 2026-09-19 at 2.57.10 PM.jpg|500]]

Switches keep track of a MAC address table, mapping physical ports on the switch to MAC addresses of devices.

![[Screenshot 2026-09-19 at 2.58.02 PM.jpg|200]]

Switches only perform 3 actions:
- Learn: records which MAC addresses are associated with which ports in MAC address table
- Flood: if it does not know the destination MAC address, it sends the frame out all relevant ports except the one it came from.
- Forward: if it knows the destination MAC address, it sends the frame only through the correct port.

Let's see examples for all 3 cases, say there's nothing in the MAC address table in the first place.


Say we have a host on port 5 of the switch, and it sent a signal to the port with L2 metadata attached:
- SRC: `a1a1`  
- DST: `d4d4`


The switch first records that MAC address `a1a1` is reachable through port 5 in the MAC address table.

It then checks its MAC address table for destination `d4d4`. If `d4d4` is unknown, the switch performs a flood: it copies the frame and sends it out every other port except the one it arrived on.

It's very similar to ARP, the switch is asking `does anyone have the mac address of d4d4?` by sending the actual frame everywhere, hoping the destination is on one of those ports.

When Host D, with MAC address `d4d4`, receives the frame, it sends data back as a response. When the switch sees that response arrive on port 8 with source MAC address `d4d4`, it learns that `d4d4` is associated with port 8 and updates its MAC address table.

![[Screenshot 2026-09-19 at 3.17.33 PM.jpg|500]]


Now, since Host D with MAC address `d4d4` is sending data to Host A with MAC address `a1a1`, and the switch already knows which ports both MAC addresses are associated with, it can perform a forward and send the frame only through the port connected to Host A.

![[Screenshot 2026-09-19 at 3.20.47 PM.jpg|500]]



Now, all of this applies between host to host communication, but also host to router communication.

And, the switch itself can be seen as a host as well, with a MAC address and an IP address.