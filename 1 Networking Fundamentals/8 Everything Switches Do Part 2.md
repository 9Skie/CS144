This is an extension of the previous lesson, where we will now look at further concepts related to switches.

- Unicast flooding: When a switch receives a frame for a specific MAC address but does not know which port that MAC address is on, it sends the frame out every other port.
    
- Broadcasts: Frames intentionally meant for every device on the local network. The switch sends them out all relevant ports.
    
- VLANs (Virtual Local Area Networks): A way to split one physical switch into multiple separate logical networks. Devices in different VLANs behave as if they are on different LANs.
    
- Multiple switches: How switches work together when a network is too large for one switch, including how they learn MAC addresses across links between switches.

---

## Unicast Flooding


Starting from last note, we recalled that we had an example frame that host A wanted to send to host D.
- SRC: `a1a1`  
- DST: `d4d4`

This is called a unicast frame, it is a frame intended for one specific destination device.

But instead, if host A wanted to broadcast a frame to all hosts on the network, it can send a broadcast frame:
- SRC:`a1a1`
- DST:`ffff.ffff.ffff`

But instead, if Host A wants to send a frame to every host on the local network, it can send a broadcast frame:

- SRC: `a1a1`
- DST: `ffff.ffff.ffff`

`ffff.ffff.ffff` is the broadcast MAC address. When a switch receives a frame with this destination, it forwards the frame out every other port in that VLAN, so all hosts on the local network receive it.



## VLANs

A VLAN lets one physical switch behave like several separate virtual switches by grouping ports into isolated Layer 2 networks.

For example, ports 1–4 could belong to VLAN 10, while ports 5–8 belong to VLAN 20. Devices in VLAN 10 can communicate with each other normally, and devices in VLAN 20 can communicate with each other normally, but traffic does not cross between the two VLANs by default.

It's like making virtual mini-switches in a big switch. 

![[Screenshot 2026-09-19 at 3.52.33 PM.jpg|500]]

![[Screenshot 2026-09-19 at 3.52.10 PM.jpg|500]]

Devices in different VLANs still use the same switch, but they are isolated from each other unless traffic is explicitly routed between them.



## Multiple Switches

But... why bother virtually dividing a switch into multiple mini-switches if we can literally just have more switches?

A single switch has physical limits. It only has a certain number of ports, and it can only handle a certain amount of traffic at once. As a network grows, one switch may no longer be enough, so we connect multiple switches together to support more devices and spread traffic across the network.

I mean, each switch individually isn't going to change, they will independently have mac address tables, independently be able to perform learn, flood, and forward operations, it's just communication between hosts on different switches become more complicated.


![[Screenshot 2026-09-19 at 4.07.32 PM.jpg|500]]

Host A, with MAC address `aaaa.aaaa.aaaa`, wants to send data to Host B, with MAC address `bbbb.bbbb.bbbb`, but the two hosts are connected to different switches. So how does the frame reach Host B?

At first, both switches have empty MAC address tables. Host A sends the frame to the blue switch. The blue switch looks at the source MAC address and learns that `aaaa.aaaa.aaaa` is connected to port 1, so it records that in its MAC address table.

The blue switch then checks the destination MAC address, `bbbb.bbbb.bbbb`. Since it does not know which port leads to Host B, it performs unicast flooding and sends copies of the frame out every other port. One copy goes to Host C, which ignores it because the destination MAC address is not its own. Another copy goes to the green switch.

When the green switch receives the frame, it sees that the source MAC address is `aaaa.aaaa.aaaa`, so it learns that Host A is reachable through the port connected to the blue switch. The green switch also does not yet know where `bbbb.bbbb.bbbb` is, so it performs unicast flooding of the frame out its other ports.

Host D receives a copy and ignores it. Host B receives a copy and accepts it because the destination MAC address matches `bbbb.bbbb.bbbb`.

When Host B sends a response back to Host A, the green switch sees the source MAC address `bbbb.bbbb.bbbb` and learns which port Host B is connected to. The response then travels toward the blue switch. When the blue switch receives it, it also learns that `bbbb.bbbb.bbbb` is reachable through the port connected to the green switch.

After this learning process, both switches know where Hosts A and B are, so future frames can be forwarded directly instead of being flooded.

---

A switch can learn MAC addresses even when those devices are not directly plugged into it. The MAC address table does not mean “this device is physically connected to this port.” It means “to reach this MAC address, send the frame out this port.”

In this example, the blue switch is directly connected to Hosts A and C, while Hosts B and D are connected to the green switch. After seeing traffic from B and D arrive through the link to the green switch, the blue switch can learn something like:

`aaaa.aaaa.aaaa → port 1`  
`cccc.cccc.cccc → port 2`  
`bbbb.bbbb.bbbb → port 3`  
`dddd.dddd.dddd → port 3`

Both B and D are associated with port 3 because port 3 is the path toward the green switch, and both of those hosts are reachable through that switch. So multiple MAC addresses can map to the same switch port.

![[Screenshot 2026-09-19 at 9.54.51 PM.jpg|500]]