This is a continuation of what we talked about from last time.

The most important concepts we need to learn about is the switch and the router, but they evolved from other earlier technology, so we will look at them first.


When 2 computers connect, they make a simple network, but when data transfers across a cable, it decays as it travels.

![[Screenshot 2026-09-17 at 11.19.07 PM.jpg|500]]

If signals fully decays before it reaches to the other side, then the 2 hosts cannot share data, to prevent this problem the repeater is made.

The repeater just regenerates signals that gets passed to it, and so signals can propagate through greater distances

![[Screen Recording 2026-09-17 at 11.21.42 PM.mov.gif|500]]



Now, consider when we have multiple hosts that connect to each other, this quickly gets out of hand.

![[Pasted image 20260917232932.png|500]]

So instead, we create a device that all hosts of a network connect to, and then it will be able to handle communication between the different hosts.

![[Screenshot 2026-09-18 at 9.38.31 AM.jpg|500]]

That's the idea of the hub, bridge, and switch.


## Hub

A hub is just a multi-hub repeater, if a host is trying to send data to another host, the hub will simply repeat the packets to every host it's connected to.

![[Screen Recording 2026-09-17 at 11.43.14 PM.mov.gif|500]]

But... does everyone need to receive everyone else's data? Not really, we'll get to that in a bit with the switch, we'll look at bridges first.


## Bridge

A bridge is simply meant to sit between hubs, by default, they can only connect up to 2 hubs (which will be upgraded by the router)

![[Screenshot 2026-09-17 at 11.49.38 PM.jpg|500]]

Bridges also learn which hosts are on each side of itself, for example, if a host is trying to communicate with another host in it's own network, the bridge will not send the dat over to the other hub, if it is in another network, the bridge will send it over.

![[Pasted image 20260917235340.png|500]]



## Switches

Switches are a combination of Hubs and Bridges, they have multiple ports, and learn which hosts are on which port.

![[Screenshot 2026-09-17 at 11.52.26 PM.jpg|500]]

So now, when a host wants to communicate with another host, the switch knows exactly which port to send the information to, and hense it will only send data to that port (that host) and not broadcast it to the entire network.

![[Screen Recording 2026-09-17 at 11.55.34 PM.mov.gif|500]]


Hosts on a network all share the same IP address space.

![[Screenshot 2026-09-17 at 11.57.22 PM.jpg|500]]



## Routers

While we see that switches can effectively handle all the communication that happens within a network, to connect networks (switches) together and facilitate communication between them, we need to use a router.

![[Screenshot 2026-09-17 at 11.59.36 PM.jpg|500]]

Routers provide a traffic control point between networks, they know which networks they are connected to (known as routes, all networks a router knows is called a routing table).

In other words, they know the IP addresses of the networks they are connected to, the router's IP address serves as the gateway for a host to communicate outside it's own network.

Look at this host, it wants to communicate with another network, it knows how to do so by talking to the default gateway, the IP address of the router itself

![[Screenshot 2026-09-18 at 12.01.38 AM.jpg|500]]

The internet is nothing more but a bunch of routers.

![[Pasted image 20260918000623.png|500]]