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
## Describe desired security properties and their applications
There are 4 security properties that we care about:
1. Confidentiality: only sender and receiver should understand message contents
2. Integrity: sender, and receiver want to confirm that the message is not altered without detection during transit
3. Authentication: sender, and receiver both can confirm the identity of one another
4. Access and Availability: internet services must be accessible and available to users
## Explain what are the possible security issues in the network / internet
1. Eavesdropping: threaten confidentiality
2. message alteration: integrity
3. impersonation: authentication
4. hijacking: authentication
5. DoS: access and availability
## Explain concepts of symmetric and asymmetric keys, as well as their pros/cons
### Symmetric Key Cryptography
Same key is used for both encrypting and decryption. i.e DES, AES
Disadvantage: Need to find a way to share the key first
Advantage: A lot faster than asymmetric
### Asymmetric Key Cryptography
Is an encryption method that uses a pair of keys for secure communication: a public key, which can be shared with everyone, and a private key, which is kept secret by the owner. Allows for digital signatures and key exchanges
Characteristics:
- Sender, receiver, do not need to pre-share secret key prior to communication
- Public encryption key know to all
- private decryption key known only to receiver
- 1024-4096-bit asymmetric key pair generated
- 1 round of encryption required
Fulfils 2 important requirements
1. Given the public key, it is impossible to compute the private key. (hard to factorise large prime numbers or solving discrete logarithm problems)
2. able to produce a public-private key pair such that it provides confidentiality and signing (authenticity and integrity)
#### Practical Uses
Secure Communications: RSA can be used to encrypt data transmissions or to secure sensitive data by encrypting it with a public key.
Digital Signatures: RSA is also used for digital signatures, where a message is signed with a sender's private key and can be verified by anyone who has access to the sender's public key.
## Explain various possible security attack scenarios depending on which cryptography techniques are or are not utilised
### Case 1
Fake message - T claiming that he's A sending a message to B
- Authentication is compromised
- Integrity is compromised
- Confidentiality is breached
### Case 2
T spoofs IP to prove that he is A
- Authentication is compromised
- Integrity is compromised
- Confidentiality is breached
### Case 3
Eavesdrop attack: eavesdropping on the messages exchanged between A and B and using the information for malicious intent.
- Authentication is compromised
- Integrity is compromised
- Confidentiality is breached
### Case 4
Now both message and "secret message" between A and B are encrypted. Even though T cannot tell what the secret message is, T can just record and playback the cipher to B.
This is called playback/replay attack.
- Intercept and re-transmit captured network data to impersonate a legitimate user
- Authentication is compromised
- Integrity is present
- Confidentiality is present

In some scenarios that require immediate critical decision making, liveness is also an important requirement.

### Case 5
Signed message + signed nonce
B wants to authenticate A and ensure the liveness of A.
Digitally sign a 1-time generated number called nonce
Still susceptible to man in the middle attack

### Case 6
Message digest: Uniquely represent the input data
After authentication is complete:
1. A sends hashed message, encrypted with A's private key
2. Plain message (if confidentiality is not an issue)
3. B hashes the plaintext message (to confirm the integrity)
4. Decrypt the encrypted hash sends by A using A's public key
5. Compare to see if it indeed is A sending the message
A hash function is required to:
1. deterministic
2. non-reversible: one-way function
	1. pre-image resistance: given a hash value, impossible to determine the input data
	2. second pre-image resistance: given the input and its hash value, impossible to find another input that produces the same hash value
3. fixed output size
4. fast computation
5. collision resistance
6. avalanche effect: small change in input data should result in very different hash value
### Case 7
If communication has to be secure, we need to use public-key cryptography + a digitally signed certificate to share a symmetric session key. Not realistic to use asymmetric key cryptography on lengthy messages. Assume an authenticated channel
1. B has to obtain A's public key from its CA-issued certificate
2. B generates a session key
3. B encrypts the session key to be sent back to A
4. Subsequent messages are encrypted using the session key

### Case 8
Secure email + non repudiation
A needs to provide a signed digest and encrypt both the message + encrypted hash together with the symmetric key. B will then:
1. Hash the received message
2. Decrypt the signed digest with CA-verified A's public key
3. Compare the two hashes to confirm message integrity
4. If matching, provides non-repudiation
## Describe how public-key certification works
1. A has to obtain a certificate from trusted CA
	- Cert contains: public key, identity information, issuer information, validity period
2. CA verifies that A exists, checks A's documents
3. CA issues a certificate that the public key of A indeed belongs to A
4. This cert is signed by the CA's private key
5. CA's public key is widely known, so we trust that nobody can impersonate the CA
6. B can obtain A's public key from the certificate by decryption it using the CA's widely known public key
## Non-repudiation
Non-repudiation is a property of a cryptographic system that ensures a party cannot deny the authenticity of a digital signature.
# Week 11
## Explain how internet naming and addressing works
1. Enter a domain name `example.com`
2. If the local DNS server knows the IP address of `example.com` it sends it back to your computer
3. If it does not know the IP address, it starts asking other DNS servers on the internet
	1. first contacts a global root server that directs it to the top-level domain for `.com`
	2. The TLD servers then point it to authoritative servers for `example.com`
4. Authoritative DNS server provides the final answer with the correct IP address
5. With the IP address provided, your computer can now connect directly to `example.com`
## Explain the difference between mnemonic names and IP addresses
## Describe what is the DNS and its purpose
DNS is an application level protocol, operating on top of the Internet Protocol Suite. It provides:
- Hostname to IP address translation
- Host aliasing: map alias names to canonical name
- Mail server aliasing
- Load distribution: replicated web servers such that many IP addresses correspond to one name
## Explain the DNS as a distributed naming infrastructure
The DNS is implemented in hierarchy of name servers, it is not centralised because:
- Need to avoid single point of failure
- avoid creating a bottleneck of traffic volume
- avoid distant centralised databases, causing long delays for lookups (DNS look-up is only name -> IP translation)
- Need to support ease of maintenance
- A centralised DNS does not scale

| Server                     | hierarchy            | J.D                                                                                                                                                                                            |
| -------------------------- | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Root Server                | highest in hierarchy | does not final answer to query, directs query to TLD. Fixed set of name servers that maintain a list of the authoritative name servers for every registered top level domain                   |
| Top Level Domain           | pretty high up there | responsible for anything after the last dot `.com`, `.edu`, `.sg`. maintained by large orgs or countries.                                                                                      |
| Authoritative name servers | middle ish guys      | provides authoritative (final, no need further query) hostname to IP mappings for organization's named hosts. Maintained by organisation itself. Or internet provider / cloud service provider |
| Local DNS                  | pretty low           | AKA DNS resolvers are used by DNS clients to resolve domain names to IP addresses. Proxy for hosts to make DNS queries, and forward queries to the hierarchy. Includes local cache.            |

## Describe design principles of the DNS infrastructure and query
- **Hierarchical Organization**: The hierarchical arrangement of root servers, top-level domains, and authoritative name servers ensures efficient and organized domain name resolution.
- **Edge Complexity**: By placing complexity at the network’s edge, DNS remains scalable and effective in handling the growing demands of the internet.
We can also conclude that DNS provides indirection (name to IP address) as its core design principle. This has **many** virtues:
- **Late binding at runtime**, e.g: physical server can move around with different IP and keeping the same name
- **Many-to-one mapping**: aliasing. Some people use _multiple_ domains aliased to a single site as part of their search engine strategy.
- **One-to-many mapping**: the same domain name can have many IP addresses. This is useful for load balancing.
### Iterative Query
In an iterative DNS query, the DNS client allows the DNS server to return the _best_ answer it can. If the queried server does not have the answer, instead of asking other servers itself, it returns a referral to other more authoritative DNS servers.

Most of the work is done by the DNS client.

### Recursive Query
A recursive DNS lookup is where one DNS server communicates with several other DNS servers to hunt down an IP address and return it to the client. The DNS server does all the work of fetching the requested information. This is in contrast to an iterative DNS query, where client communicates directly with each DNS server involved in the lookup

The DNS server does all the work of fetching the requested information. Typically supported

The type of DNS query is determined by the client making the query and the configuration of the DNS server receiving the query

### Protocol
![[Pasted image 20240814180723.png]]
Identification: 16-bit number for query, reply uses same number
Flags:
- Query or Reply? `qr` means query response (reply)
- Recursion desired? `rd` 
- Recursion available? `ra`
- Reply is authoritative? `aa`
Size: varies depending on number of RRs (resource-records) in each section
Components of variable lengths of DNS query / reply:
- Questions: name, type fields for a query
- Answers: RRs in response to the query
- Authority: Records for authoritative servers
- Additional information: possibly helpful info
## Explain different types of DNS records

| Name                                   | Value                                                         | Type  | TTL       |
| -------------------------------------- | ------------------------------------------------------------- | ----- | --------- |
| Hostname, e.g. sutd.edu.sg             | IP Address                                                    | A     | N seconds |
| Domain, e.g. sutd.edu.sg               | the name server that is authoritative to this domain          | NS    | N seconds |
| Alias name for some canonical hostname | real hostname                                                 | CNAME | N seconds |
| Domain, e.g. sutd.edu.sg               | mailserver name, e.g: sutd-edu-sg.mail.protection.outlook.com | MX    | N seconds |

## Explain the purpose of DNS caching and how it is done as well as maintained over time
The Local NS will cache hostname-ip mapping once a query is made for a certain TTL. TLD servers are typically cached in DNS local name servers. Allowing for faster resolution.
### Handling Cache Update
The authoritative name server decides the DNS record TTL. Local nameserver re-queries when TTL expires.

### DNS record insertion
1. Find a DNS registrar to register the domain name of your site
2. Find authoritative nameservers that will be responsible for solving your domain name
3. Insert DNS records (A type) into the authoritative nameservers
4. Insert DNS records (NS types) of your authoritative nameservers to the TLD
## Explain how DNS lookup and dig works, as well as their applications
### `nslookup`
#### obtaining an authoritative answer
1. Find the NS-type record of `google.com` using `nslookup`
```sh
nslookup -type=ns google.com
```
2. query one of the outputs about the IP address of `google.com`
```sh
nslookup google.com ns1.google.com
```
`nslookup` has to resolve `ns1.google.com` first (to obtain its IP address), and then resolve `google.com` by sending a DNS query to `ns1.google.com`. You can also do this manually

### `dig`
1. header: `id: 22259`
2. flags
	- `qr` : query response
	- `rd` recursion desired
	- `aa` authoritative answer
	- `ra` recursion available
3. next numbers of queries answers, authority and additional section
4. Answer section
	1. each column in answer indicates TTL(s),class, type, and value

# Week 12
## Explain how basic socket programming works
GOAL: Pass the packet from the internet to the application that needs it e.g. steam, web browser

We can name application endpoints using sockets. It is a application layer protocol.

A socket is an endpoint for sending or receiving data across a computer network. It is a 16 bit unsigned integer as a combination of:
- IP address
- port number
Together they identify a specific process on a specific machine and is unique. They serve as the programming interface between the application layer and the transport layer. 

There are 2 types of sockets
1. stream sockets (TCP): connection-oriented communication. Reliable, ordered, error-checked
2. Datagram sockets (UDP): connection-less communication. Faster, less reliable, no order, no error-checking
Socket is like a door between application process and end-end transport layer protocol.

### Transport Layer
The transport layer protocol multiplex at sender and de-multiplex at receiver
- Multiplexing: combining data from multiple application processes into a single stream of packets that can be sent over the network. TL adds necessary headers (source and destination port numbers) to each packet before going to network layer
- Demultiplexing: TL receives packets from the network layer and directs them to the appropriate application process.
## Explain how UDP and TCP works
### TCP
TCP proves reliable, in-order byte-stream transfer between client and server in application viewpoint. It involves a three-way handshake process to establish a connection, ensuring the parties are ready to communicate. It is identified by a 4 tuple: source IP address, source port number, destination IP address, destination port number
#### Connection establishment
1. Server process must be running, create a socket that welcomes client's contact
2. Client must contact server by creating TCP socket, specifying IP and port # of server host process
3. Server that's contacted by client must create a new socket for server process to communicate with that particular client
4. Server process can continue accepting more clients, while communicating with multiple clients (each with different socket)
#### Three-way handshake
Happens during connection establishment
1. `SYN`
2. `SYN-ACK`
3. `ACK`

## Describe the main differences between UDP and TCP, as well as their applications, pros/cons
## Explain client-server model of network applications
## Explain how web server works and the HTTP requests and responses
## Explain how cookies store user-server state and its function
## Analyse how web caching impacts performance
## Explain how wireshark packet sniffer and analyser works and its application