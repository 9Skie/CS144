Now, we will continue onwards with the OSI model.


4. Transport Layer

The Transport Layer handles communication between applications running on hosts. While the Network Layer gets data to the correct computer using IP addresses, the Transport Layer gets that data to the correct program using port numbers.

For example, your PC can be running a game, a browser, and discord at the same time, but all their network traffic comes and goes through this 1 IP address, the question then is: how can we make sure the right applications receive the right packets?

![[Screenshot 2026-09-19 at 10.01.59 AM.jpg|500]]

The two main protocols are TCP and UDP. TCP focuses on reliable delivery, while UDP is faster and has less overhead but does not guarantee delivery.

The basic idea is that, each application has a different port number on your computer, then when data arrives on the wire, it's going to include meta data about which port it's going to on the computer.

![[Screenshot 2026-09-19 at 10.06.13 AM.jpg|500]]


Now, how does a client connect to a server? With our knowledge, we can answer this question now.

Suppose you have three HTTPS connections open. Your computer chooses a different temporary client port for each connection, while each server continues listening on port 443. It might look like:
- `Your PC: 192.168.1.10:51001 → Website A: 142.x.x.x:443`
- `Your PC: 192.168.1.10:51002 → Website B: 104.x.x.x:443`
- `Your PC: 192.168.1.10:51003 → Website C: 172.x.x.x:443`

So if you log into Website A, the packet has your IP address and temporary port as the source, and the website server’s IP address and port 443 as the destination:
- `source = your IP : 51001`
- `destination = server IP : 443`

The server replies in the opposite direction:
- `source = server IP : 443`
- `destination = your IP : 51001`

This works because a connection is identified by the combination of source IP, source port, destination IP, and destination port. That is why thousands of clients can all connect to the same server on port 443 without interfering with one another.

![[Pasted image 20260919104700.png|500]]


5. The rest of the layers

These layers I believe are less relevant to what we want. Layers 5–7 are much more about how applications communicate once the data has already reached the correct process.

In real systems, these upper three layers are often not cleanly separated. They are usually implemented inside applications themselves and have no universal standard.

![[Screenshot 2026-09-19 at 10.56.34 AM.jpg|300]]

---

Now that we understand the entire OSI model, we can go understand how the back and fourth of data transmission works for 2 hosts.

When one host sends data to another, the data is encapsulated as it moves down the networking stack.

At the application layer, we start with the actual application data. The transport layer adds its own header, producing a segment. The network layer adds an IP header, producing a packet. The data link layer adds a frame header and trailer, producing a frame. Finally, the physical layer sends that frame as bits over the physical medium.

- Application layer: actual application data  
    - `-DATA`
    
- Transport layer: adds TCP or UDP information  
    - `[TCP/UDP][DATA]` → segment
    
- Network layer: adds IP information  
    - `[IP][TCP/UDP][DATA]` → packet
    
- Data Link layer: adds MAC information  
    - `[MAC][IP][TCP/UDP][DATA]` → frame
    
- Physical layer: converts the frame into bits
    - `010101101001...`

When the receiving host gets the bits, the process happens in reverse. Each layer removes the information added by its matching layer, this is called decapsulation.

![[Screenshot 2026-09-19 at 11.07.46 AM.jpg|500]]

---

## Conclusion

The OSI model organizes network communication into layers, with each layer handling a specific responsibility.

1. The application layers produce the original data. 
2. The Transport Layer adds TCP or UDP information such as port numbers, creating a segment. 
3. The Network Layer adds IP addressing, creating a packet. 
4. The Data Link Layer adds MAC addressing, creating a frame. 
5. The Physical Layer then transmits that frame as bits through a physical medium.

The lower layers also correspond to different kinds of communication. 
- Transport handles application-to-application communication, 
- Network handles host-to-host communication across networks
- Data Link handles hop-to-hop communication on a local link
- Physical handles the actual transmission of bits.

![[Screenshot 2026-09-19 at 11.08.12 AM.jpg|500]]