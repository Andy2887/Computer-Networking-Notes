# Network Layer

---

## IPv4 Addressing

**Interface**: An interface is the connection between a device (like your computer or a router) and a physical network link.

Routers usually have multiple interfaces (for multiple links).

Hosts usually have one or two interfaces (WiFi + wired).

### IP Subnet

Subnets are chunks of IP addresses in that can communicate directly, without traveling through a router.

In the graph below, a blue area is a subnet.

<img src="/Users/yuanliheng/Desktop/CS340/notes/assets/subnet.jpg" style="zoom:50%;" />

**Subnet Mask**: A subnet mask is used to define where the division occurs between the part of the address that identifies the network and the part that identifies the specific device (host).

There are two common ways to express this:

- **CIDR Notation (Slash Notation):** For example, in **223.1.1.3/24**, the "**/24**" indicates that the first **24 bits** of the address identify the subnet, while the remaining **8 bits** identify the individual host.

- **Bitmask Notation:** The same "/24" range can be written as **255.255.255.0**. (The first 24 bits are 1, and the last 8 bits are 0).

### The Evolution of IP Addressing

**Early Scheme**: class A, B, C subnets (/8, /16, /24-bit subnets)

Example: Assume Northwestern needs to buy an subnet of 24 bits, then they get 256 addresses.

**Nowadays**: Classless InterDomain Routing (CIDR) allows for flexibile bit of subnets.

### Forwarding Rules

Forwarding rules help the router decide which link the packet should be sent out on.

Billions of IP addresses are possible, so rules apply to ranges of addresses.

<img src="assets/forwarding_rule.jpg" style="zoom:50%;" />

Forwarding tables in IP use longest prefix matching. Specifically, they check whether the IP address matches the pattern in the column "Destination Address Range".

If more than one rule matches, choose the rule with longest prefix.

<img src="assets/longest_prefix.jpg" style="zoom:50%;" />

### IPv4 Packet (datagram)

<img src="assets/datagram.jpg" style="zoom:50%;" />

**IPv4 fragmentation**: Large IP datagrams may be fragmented anywhere, and reassembled at final destination.
