# Week 8

## The internet
### Explain what a network is and the internet
### "Nuts and Bolts" vs "Service"
#### "Nuts and Bolts"
The internet is essentially made of hosts/end systems, communication links, and packet switches.
	End System: Devices running network applications
	Communication Links: Cables, bandwidth (transmission rate in bits/second)
	Packet Switches: Takes a packet arriving on one of its incoming communications links and forwards that packet on one of its outgoing communication links
#### "Service"
The alternative definition of the internet is an infrastructure that porvides services to applications
Providing BOTH physical infrastructure, but also programming interfaces for these applications to use
### Discuss challenges for the internet
Challenges include:
- Operability: How do the independent devices communicate and synchronize, what medium or protocols do they use?
- Sharing: How do we control traffic of packets? How do we share resources between N hosts that wish to communicate with one another?
- Complex interacting components: How do we manage the interaction between so many different types of devices?
- Scalability: How do we support the growing number of devices?
### Internet Structure
#### Network Layering
Network layering is a design principle in computer networking where each layer of a network architecture performs specific functions and only interacts with the layers directly above and below it.

Modules could refer to: 
- Routers and switches, which direct data traffic.
- Servers, which store and deliver content such as websites
- Transmission media, such as cables and wireless signals that carry the data
- Protocols, such as TCP/IP, which define the rules for transmitting data.
Modularity is not the answer because of the amount of interaction between modules. Hence layering.

Layering:
- Reduces interactions between sub componenst
- Each layer relies on the services of the layer directly below it
- With N layers, there's only N-1 interactions we need to consider
#### Draw the internet structure
The 5 layer OSI model:
1. Application layer (software): An application layer packet of information is called a message
2. Transport layer (software): typically implemented as part of the OS networking stack (a transport layer packet of info is called a segment) think letter with a destination address.
	- TCP: Connection-oriented, guarantees delivery
	- UDP: Connectionless, does not guarantee delivery
3. Network layer (both software and hardware): responsible for routing datagrams (path planning), network layer packet of info is called a datagram. actually delivering the segment
	1. Network layer protocols: primarily software
		- IP: Routes and addresses
		- ICMP: Diagnostics and error reporting within IP networks
		- OSPF: Determines optimal routing
		- BGP: Facilitates routing
	2. Network Layer Devices (solely made to support up to network layer):
		- Router: Forwards data packets between computer networks based on IP addresses
		- Layer 3 switch: Combines switching and routing functions at wire speed
		- Gateway: Acts as an entry or exit point between different networks, translating protocols or addressing schemes
		- Network Firewall: Filters and controls network traffic based on predefined security rules
	3. Software (implementation):
		- Routing protocol software
4. Link Layer: Provide reliable delivery service of packets from one node to another in the (already known) route. Link layer does not route, it simply delivers the packets of information to the next node already determined by the network-layer. Link layer packet is called frame
	1. Protocols (both hardware and software):
		1. Ethernet
		2. ARP (address resolution protocol): Maps IP addresses to MAC addresses for data transmission within a network
		3. PPP (Point-to-Point protocol): Establishes direct connections between two nodes over a serial link.
	2. Devices:
		1. Ethernet switch: Forwards data frames between devices within a LAN based on MAC addresses
		2. NIC (network interface card): connects a computer or device to a network, handling data transmission at the link layer.
		3. Wireless Access Point (WAP): connects wireless devices to a wired network, managing wireless communication at the link layer
	3. Software:
		1. NIC Driver: Implement link layer protocols such as ethernet and provide an interface for the operating system to send and receive data frames over the network.
5. Physical Layer (Purely hardware) Move individual bits within the frame from one node to the next:
	1. Protocols:
		1. Ethernet physical layer: transmission medium
		2. IEEE 802.11: Specifies the physical layer for wireless LAN, including modulation techniques and frequency bands
	2. Devices:
		1. Ethernet cable
		2. Wireless antenna
		3. Ethernet Transceiver
		4. Fiber Optic Cable

#### Explain the concept of priority inversion and emergent behaviour
Priority Inversion: Something of higher priority to inadvertently become the lowest priority due to some misguided usage of locking mechanism.
Emergent Behaviour: Erroneous behaviours that aren't observed in the modules individually, but arise when you let one module interact with another.
## Protocols
### Explain what a protocol means
Protocol is the format and order on how to send messages through the internet and actions taken on message transmission and receipt.
### Explain the difference and pros/cons between time domain multiplexing, frequency domain multiplexing, and packet switching
#### Circuit Switching
Advantage: Guaranteed stable connection for that reserved slot / bandwidth
Drawback: Expensive, under utilisation is possible. User i cannot utilise resources that user j has in the other time slot of frequency band.
#### Multiplexing the circuit
Goal: share the resources. Two methods:
1. Frequency division multiplexing (FDM): each circuit continuously gets a fraction of the bandwidth (e.g 4KHz)
2. Time division multiplexing (TDM): each circuit gets all of the link bandwidth periodically during brief intervals of time slots
![[Pasted image 20240728172342.png]]
Neither method is faster than the other given similar specifications
#### Packet Switching
Packet switching is a method of data transmission where messages are broken into smaller packets, sent independently over a network, and reassembled at the destination. Another way of resource sharing.
	Advantage: Better for bursty data
This is because packets occupy network resources on demand. Modern internet fundamentally relies on packet switching.

| Protocol          | When to use                |
| ----------------- | -------------------------- |
| Circuit Switching | Continuous streams of data |
| Packet switching  | Bursty data                |
### Protocol stack
![[Pasted image 20240728182200.png]]
![[Pasted image 20240728182304.png]]



# Week 9
## Describe and explain the sources of packet delay through the network

| Type of delay      | description                                                                                            | Duration                                                                                                                           | Cause                                                             |
| ------------------ | ------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| Processing delay   | time taken for router or switch to examine a packet's header and determine where to forward the packet | < ms, usually bounded or fixed                                                                                                     | Need time to examine packet header                                |
| Queueing delay     | time a packet spends waiting in a queue before it can be transmitted into network link                 | non-trivial to compute                                                                                                             | packet input rate > link output rate, need to wait in node buffer |
| Transmission delay | how fast the bits are pushed into the link                                                             | $d_{trans} = \frac{L}{R}$ where L is packet length in bits and R is link bandwidth in bits/s                                       | Need time to push whole packet bits from router to link           |
| Propagation delay  | time taken for signal to travel from the sender to the receiver over a given medium                    | $d_{prop} = \frac{d}{s}$ where d is link length in metres, and s is the propagation speed of bits on a medium, $2 \times 10^8$ m/s | EM wave takes time to travel                                      |
| Total nodal delay  | sum of all types of delay                                                                              | $d_{nodal} = d_{proc} + d{queue} + d_{trans} + d_{prop}$                                                                           |                                                                   |
### Average Link Utilisation
![[Pasted image 20240730164302.png]]
Average queuing delay is related to the average link utilisation.
$$
U = \frac{La}{R}
$$
Where,
- $L$ is packet length in bits
- a is average packet arrival rate (packets/sec)
- R is the link's bandwidth in bits/s
**do not try to use every bit of the link capacity**
## Compute effective throughput between two end systems
## Explain how traceroute and ping works, as well as their applications
### Traceroute
traces the path that data packets take from one point to another on a network. Relies on ICMP (network layer protocol) to work. But must be encapsulated in IP packets (at the "top" of network layer)

![[Pasted image 20240730165526.png]]
At each probe $i$, traceroute sends 3 packets that will reach router $i$ on path toward destination. Each router will reduce TTL by 1. When TTL = 0, router returns ICMP TTL exceeded packet, because it is not the destination.

# Week 10
## Explain what are the possible security issues in the network / internet
## Describe desired security properties and their applications
## Explain basic cryptography techniques
## Explain concepts of symmetric and asymmetric keys, as well as their pros/cons
## Explain various possible security attack scenarios depending on which cryptography techniques are or are not utilised
## Describe how public-key certification works
# Week 11
## Explain how internet naming and addressing works
## Explain the difference between mnemonic names and IP addresses
## Describe what is the DNS and its purpose
## Explain the DNS as a distributed naming infrastructure
## Describe design principles of the DNS infrastructure and query
## Explain different types of DNS records
## Explain the purpose of DNS caching and how it is done as well as maintained over time
## Explain how DNS lookup and dig works, as well as their applications

# Week 12
## Explain how basic socket programming works
## Explain how UDP and TCP works
## Describe the main differences between UDP and TCP, as well as their applications, pros/cons
## Explain client-server model of network applications
## Explain how web server works and the HTTP requests and responses
## Explain how cookies store user-server state and its function
## Analyse how web caching impacts performance
## Explain how wireshark packet sniffer and analyser works and its application