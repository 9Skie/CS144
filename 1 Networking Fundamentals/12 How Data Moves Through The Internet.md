This is the summary lesson of everything we've learnt so far! We known about OSI, switches, routers, MAC address tables, ARP tables, and routing tables... look how far we came!

Hm... actually, this video was like... a huge example for what happens when you type `www.google.com` into the web browser.

---
 
We start from our host computer, which has its own IP address and MAC address. When we type `www.google.com` into the browser, the computer does not initially know which IP address corresponds to that domain name. However, it already knows the IP address of a DNS resolver, usually provided through our network configuration.

Our computer therefore sends a DNS request. At the Transport Layer, the operating system chooses a temporary source port for this request, while the DNS server listens on port `53`. So we might have:

`my computer: 52000 → DNS server: 53`

At Layer 3, the packet contains:

`source: my computer's IP`  
`destination: DNS server's IP`

Our computer checks the destination IP and realizes that the DNS server is not on the local network. Therefore, the packet must first be sent to our default gateway.

However, we cannot construct the Layer 2 frame yet because we need the router’s MAC address. We already know the router’s IP address from our default gateway configuration, so we send an ARP request through the switch asking for that IP’s MAC address. The switch floods the request across the local network, and the router responds with its MAC address.

Our computer stores this mapping in its ARP table and constructs the Layer 2 frame:

`source: my computer's MAC`  
`destination: router's MAC`

The switch receives the frame, looks at its MAC address table, and forwards it to the router.

The router removes the old Layer 2 header and examines the destination IP address. It searches its routing table using longest-prefix matching to determine the next hop. If no more specific route exists, it may use its default route toward the Internet.

From this point, every router repeats the same basic process:

1. Look at the destination IP.
2. Choose the next hop using the routing table.
3. Get the next-hop MAC address from the ARP table, performing ARP if necessary.
4. Build a new Layer 2 frame.
5. Send.

Eventually, the packet reaches the network containing the DNS server. The final router determines that the DNS server is directly reachable. If it does not already know the DNS server’s MAC address, it performs ARP, builds the final Layer 2 frame, and sends the packet to the DNS server.

The DNS server receives the request on port `53`, determines the IP address associated with `www.google.com`, and sends the answer back to the temporary port our computer used for the DNS request.

Now our browser finally knows Google’s IP address and can begin communicating with Google itself. The operating system chooses another temporary source port, while Google’s HTTPS server listens on port `443`:

`my computer: 52001 → Google server: 443`

At Layer 3, we now have:

`source: my computer's IP`  
`destination: Google's IP`

The packet again travels through our switch, router, and then router by router across the Internet using the same forwarding process until it reaches Google’s network.

Once the connection reaches Google’s server, the browser communicates with the HTTPS service on port `443` and requests the webpage. Google sends the response back toward our computer. The networking stack uses the destination IP address to get the data back to our computer and the destination port to deliver it to the correct application, in this case, our browser.

![[Screenshot 2026-09-20 at 3.40.17 PM.jpg]]

