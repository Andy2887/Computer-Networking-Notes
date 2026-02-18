# Network Layer

---

## IPv4 Addressing

**Interface**: An interface is the connection between a device (like your computer or a router) and a physical network link.

Routers usually have multiple interfaces (for multiple links).

Hosts usually have one or two interfaces (WiFi + wired).

### IP Subnet

Subnets are chunks of IP addresses in that can communicate directly, without traveling through a router.

In the graph below, a blue area is a subnet.

<img src="assets/subnet.jpg" style="zoom:50%;" />

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

---

## Network Address Translation (NAT)

**Network Address Translation (NAT)** is a method used to remap one IP address space into another while they are in transit across a traffic routing device.

The **NAT router** makes your entire local network look like **one big machine** to the outside world. Every device on your local network gets a private IP address (like 10.0.0.12, 10.0.0.3, etc.), but when packets leave your network, the router rewrites them to use its single public IP address with different port numbers. Then NAT will store the address mapping.

*Example:*

Say your laptop is 10.0.0.1 on the local network, and your router's public address is 138.76.29.7.

Your laptop sends a packet to a web server. The NAT router intercepts this. It picks an unused public port — say 5001 — and **rewrites the source of the packet** to 138.76.29.7 port 5001.

**Pros:**

**Security** — your local devices are invisible to the outside world.

**Configuration isolation** — if your ISP changes your public IP, nothing on your local network needs to change.

**Cons:**

The NAT only creates a port mapping when a local device sends a packet *out*. If someone on the Internet tries to reach your machine cold, the NAT has no mapping for them and just drops the packet. You need **port forwarding** — manually telling your router "anything that comes in on port 8080, send to 10.0.0.5."

---

## IPV6

The REAL fix for address exhaustion is **IPv6**. Instead of 32-bit addresses, IPv6 uses **128 bits**. It is written in hex, eight groups of four hex digits. Example: `a39b:239e:ffff:29a2:0021:20f1:aaa2:2112`

There are shorthand rules: you can replace consecutive groups of all zeros with `::` (but only once), and you can drop leading zeros in each group. So `0000:0000:0000:0000:0000:0000:0000:0001` becomes just `::1` — that's localhost.

### IPv6 Header Improvements

<img src="assets/ipv6_header.jpg" style="zoom:50%;" />

**No Checksum.** Because TCP/UDP already have checksums above, and Ethernet has one below. And since TTL (now called "hop limit") gets decremented at every router, the checksum had to be recalculated at every hop. 

The TTL (Time to Live) field is *part of the IPv4 header*. Every single router that forwards the packet decrements TTL by 1 — that's how we prevent packets from looping forever. But the moment you change even one bit of the header, the old checksum is no longer valid. So every router has to: decrement TTL, then **recompute the checksum over the entire header.**

**No Fragmentation.** IPv4 routers could fragment oversized packets. IPv6 says: routers should be simple and fast, let endpoints deal with fragmentation (TCP).

---

## Making IPv4 and IPv6 Coexist

### Tunneling

If you have two IPv6 "islands" separated by IPv4-only routers, you wrap the IPv6 packet inside an IPv4 packet, send it across the IPv4 network, and unwrap it on the other side.

![](assets/tunneling.jpg)

### Dual-stack

**Dual-stack** hosts have both an IPv4 and IPv6 address. When connecting to a server, they do both a DNS A record lookup (IPv4) and a AAAA record lookup (IPv6). If the server has both, the client can pick.

<img src="assets/sum.jpg" style="zoom:50%;" />

---

## Router

Think of a router abstractly as: **input ports → a queue → output ports.** Packets arrive on input ports, wait their turn, and leave on output ports. 

### The Switching Fabric

The internal interconnect between input and output ports.

**Bus-based:** Simple. Only one packet can cross at a time.

**Crossbar:** Multiple flows can cross simultaneously. If you have *n* ports, you need *n²* switch points, so it's expensive. But it lets input port 1 send to output port 3 *at the same time* that input port 2 sends to output port 4.

<img src="assets/bus_crossbar.jpg" style="zoom:50%;" />

### Weighted Fair Queuing (WFQ)

Instead of a single FIFO queue, we have *multiple* queues with different priorities.

An administrator classifies packets by header fields — source/destination IP, port number, protocol — and assigns each class a weight.

<img src="assets/wfq.jpg" style="zoom:50%;" />

### Routing Algorithms

**Centralized (Link-State) algorithms:** Every router knows the entire network topology. Each router broadcasts its local link information to everyone, then independently computes shortest paths. Example: **OSPF**.

**Distributed algorithms:** Each router only knows its neighbors and gradually learns about the rest through iterative information sharing. Harder to design correctly, but scales better across organizational boundaries. Example: **BGP**.

### Bellman-Ford vs. Dijkstra — When to Use Which?

|                 | Bellman-Ford                           | Dijkstra                       |
| --------------- | -------------------------------------- | ------------------------------ |
| Speed           | Slower: Θ(\|V\|·\|E\|)                 | Faster: Θ(\|E\|+\|V\|log\|V\|) |
| Negative edges? | Handles them (detects negative cycles) | Cannot                         |
| Implementation  | Naturally distributed                  | Best centralized               |
| Used by         | **BGP** (Distance Vector)              | **OSPF** (Link State)          |

### The Distance Vector (DV) Algorithm

This is Bellman-Ford adapted for a *distributed* setting. Each router maintains a **distance vector** — its current best estimate of the cost to reach every other router.

The protocol is beautifully simple:

1. **Initially**, each router only knows the cost to its direct neighbors.
2. **Send** your distance vector to all neighbors.
3. **When you receive** a neighbor's DV, recalculate your own using the Bellman-Ford equation: d_x(y) = min over all neighbors v of {cost(x,v) + d_v(y)}.
4. **If your DV changed**, send the update to your neighbors.
5. **Repeat** until nothing changes — the algorithm has converged.



