Routers aren't usually connected randomly or in equal status fashion to each other, they are usually organized in a hierarchy, for the purpose of easily scaling.

For example, if department routers were connected in a long chain, the Accounting network might need to pass through several routers before reaching the Internet. 

![[Screenshot 2026-09-20 at 2.39.56 PM.jpg|300]]

Instead, each department router can connect to a higher-level router, which then connects to the Internet. The main benefit is scalability and simpler routing, with shorter paths being a common consequence of the design.

![[Screenshot 2026-09-20 at 2.40.13 PM.jpg|300]]



We could even merge the example network from last note to this note as well, look at how much cleaner the topology is just as a consequence of hierarchical router topology! 

![[Screenshot 2026-09-20 at 2.45.15 PM.jpg|500]]


Anyways, hierarchical routing allows for route summarization, which means combining several smaller network routes into one larger route.

Consider in our diagram, this would be currently `R5`'s routing table.

![[Screenshot 2026-09-20 at 2.50.15 PM.jpg|300]]


Now, realize how all the bottom three routes point to `R4`. We can simplify them into a single summarized route:

`10.20.0.0/16 → R4`

This tells `R5`: “Any destination beginning with `10.20.x.x` should be sent to `R4`.” So instead of storing many individual routes, `R5` only needs one broader route.

![[Screenshot 2026-09-20 at 2.58.34 PM.jpg|300]]


Now we can finally return to something we have been using for a while: subnetting, especially notation like `/24` and `/16`.

The number after `/` tells us how many bits of the IP address belong to the network prefix.

For example:
- `/24` → match the first 24 bits → first 3 octets  
-  `/16` → match the first 16 bits → first 2 octets

![[Screenshot 2026-09-20 at 2.57.10 PM.jpg|500]]

So `10.20.55.0/24` means addresses beginning with `10.20.55.x`, while `10.20.0.0/16` means addresses beginning with `10.20.x.x`. This is why `/16` can summarize many different `/24` networks under one larger route.



This can be taken further, take this naive routing table for `R8`. 

![[Screenshot 2026-09-20 at 3.01.32 PM.jpg|300]]


Knowing how subnetting works, we can first combine the three `10.20.x.x` routes into one broader route:

`10.20.0.0/16 → R5`

This works because all three networks share the same first 16 bits, so R8 does not need a separate entry for each `/24` network.

![[Screenshot 2026-09-20 at 3.02.54 PM.jpg|300]]


We can simplify even further. Since every network except R8’s directly connected `10.40.77.0/24` network is reached through R5, we can use:

`10.0.0.0/8 → R5`

![[Screenshot 2026-09-20 at 3.03.10 PM.jpg|300]]


But now, say a packet of dat came with the destination being `10.40.77.x`, which route should the router `R8` take?

Routers use longest-prefix matching: among all matching routes, they choose the most specific one. So `10.40.77.0/24` is chosen over `10.0.0.0/8`.


We can simplify even further. If every destination that is not directly connected to `R8` should be sent to `R5`, we can use a default route:

`0.0.0.0/0 → R5`

`/0` matches every IPv4 address. More specific routes still take priority, so `10.40.77.0/24` is used for the local network, while everything else goes to `R5`.
- Which is true!

![[Screenshot 2026-09-20 at 3.12.24 PM.jpg|300]]