Have you ever wondered how the internet, one of the greatest inventions in the 20th century worked?

Say we were playing a multiplayer game like CS:GO, and I shot another player. The communication path can be split into three parts: my local network, the Internet, and their local network.

On **my local network**, my PC creates network packets and sends them through my switch, then to my router, and to my ISP's (internet service provider) network.

Now these packets are on **the Internet**. They travel through a series of routers operated by ISPs and other networks, with each router forwarding the packets closer to the CS:GO server. The server receives the packets, processes what happened in the game, and sends updated game data back out through the Internet toward the other player.

That data travels through another series of routers until it reaches the other player’s router, at that point, they enter **their local network**. From there, the packets travel through their switch, and finally arrive at their PC.

Boom! Headshot!

![[Pasted image 20260917225340.png|500]]


---

Now, you understand the big gist of networks, but how does each individual component work in this model? We'll look at them now.

## Hosts

Hosts are any device which send or receive network traffic, like in our example, my/their pc, but this could go much broader, many electronic devices are hosts.

Now, hosts can be divided into clients and servers.
- clients initiate requests
- servers respond requests

![[Pasted image 20260917225652.png|500]]

Servers are simply computers with software installed which responds to specific requests.


## IP Address

Just like how we have phone numbers to call each other, or email addresses to send and receive email, we need IP addresses to receive and send network packets, it's the identity of each host.

Whenever a request is made from a client, the data will contain information about the source IP address and destination IP address, this is true vice versa for the server response as well.

![[Screenshot 2026-09-17 at 10.59.53 PM.jpg|500]]


An IPv4 address is written as four decimal numbers separated by dots, like `192.168.1.42`. Each number represents 8 bits, so each one can range from `0` to `255`. Altogether, an IPv4 address is 32 bits long.

The address is divided into a **network portion** and a **host portion**. For example, with the network `192.168.1.34`, the first 24 bits identify the network, while the final 8 bits identify a particular host inside that network, 34.

So the hierarchy can be thought as:
- IP address = network prefix + host identifier


## Network

A network is what actually transports traffic between hosts, before we had the concept of the internet, transferring data between hosts would mean we would need to carry a hard disk over from a computer to another network!

All network interfaces whose IP addresses share the same **network prefix** are considered part of the same IP network, or **subnet**.

For example, in `192.168.1.x`, addresses from that prefix belong to the same subnet, such as `192.168.1.10` and `192.168.1.42`.

Networks can contain other networks, called subnets, and the connection of networks to other networks is called the internet.

