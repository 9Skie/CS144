We will understand a few scenarios for how communication happens for hosts over the internet.

1. 2 hosts are communicating to each other on the same network
2. 2 hosts are communicating to each other on different networks

![[Screenshot 2026-09-19 at 2.21.17 PM.jpg|500]]

This note is specifically for communication on the same network.


Consider we have 2 hosts in the same network, and host A wants to send it to host B
- Host A knows the IP of host B, and vice versa. Based on that information, host A knows that host B is within it's same IP network due to subnetting.
- But host A doesn't know host B's mac address.

![[Screenshot 2026-09-19 at 2.24.04 PM.jpg|500]]


So, how does host A find out host B's MAC address through host B's IP address?

Host A uses ARP, the Address Resolution Protocol. Host A first checks whether it already knows Host B’s MAC address in its ARP cache. If not, it sends a broadcast message to the local network asking:

`Who has IP 10.1.1.33? If Anyone has this IP, please send me your MAC address`

Every device on the local network receives the request (In this process, host A also broadcasts it's IP and MAC address), but only the device with that IP responds. Host A then stores that mapping in its ARP cache and can send Ethernet frames directly to Host B’s MAC address.

![[Screenshot 2026-09-19 at 2.26.25 PM.jpg|500]]

Since the ARP request is a broadcast that sends itself to everyone on the network, the destination MAC address for an ARP request is `ffff.ffff.ffff`.


Once host B receives the message, it responds by sending an ARP response directly to host A, with host A's IP and MAC addresses as the destination addresses.

![[Screenshot 2026-09-19 at 2.31.20 PM.jpg|500]]

