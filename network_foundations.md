# Network Foundations

---

## Introduction

### What is the Internet?

The Internet is a **"network of networks"** consisting of billions of computing devices (hosts/end systems) interconnected globally.

---

## Internet Architecture

### Network Edge

The **network edge** includes all hosts (clients and servers) that connect to the Internet:
### Network Core

The **network core** is a mesh of interconnected routers that relay packets between different parts of the Internet:
### Access Networks

Access networks connect end users to the Internet:

| **Type** | **Description** | **Characteristics** |
|----------|----------------|---------------------|
| **DSL (Digital Subscriber Line)** | Uses existing telephone lines | Data and voice transmitted at different frequencies |
| **Cable** | Uses coaxial cable distribution networks | Originally designed for TV; shared among neighborhoods |
| **Wireless** | Radio-based connectivity | WiFi (short range), Cellular (wide area), Satellite |

---

## Network Components

### Hosts

- Devices connected to the network (smartphones, servers, thermostats, etc.)
- Each host has a **unique IP address**
- Also called **end systems**

### Communication Links

Physical media that transmit data:
- **Copper wire**: Traditional, cost-effective
- **Fiber optic**: High-speed, long-distance
- **Radio**: Wireless transmission

### Switches

- Connects multiple devices within a single **Local Area Network (LAN)**
- Forwards data between devices on the same network

### Routers

A router is the gateway that connects your local network to the **Internet**.

**Two Key Responsibilities:**

1. **Distributed Routing Algorithms**
   - Determine which address ranges are most quickly reachable on each outbound link
   - Build and maintain routing tables

2. **Packet Forwarding**
   - Direct packets according to routing decisions
   - Forward packets to the appropriate next hop

![Router Architecture](assets/routers.jpg)

---

## Switching Methods

### Circuit Switching vs. Packet Switching

| **Feature** | **Circuit Switching** | **Packet Switching** |
|------------|----------------------|---------------------|
| **Method** | Dedicated end-to-end path established for each call | Data sent in small chunks (packets) |
| **Efficiency** | Can be wasteful during silence; "all or nothing" | Highly efficient for "bursty" traffic |
| **Performance** | Performance is guaranteed | "Best effort" delivery; no guarantees |
| **Usage** | Traditional telephone networks (PSTN) | Modern Internet |
| **Resource Allocation** | Resources reserved for duration of connection | Resources shared dynamically |

---

## Network Performance

### Four Sources of Nodal Delay

Total nodal delay: $d_{nodal} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$

1. **Processing Delay ($d_{proc}$)**
   - Checking for bit errors
   - Determining output link (figure out where to go)
2. **Queueing Delay ($d_{queue}$)**
   - Time waiting at output link queue
   - If the finite-sized queue is full, **packets are dropped**
3. **Transmission Delay ($d_{trans}$)**
   - Formula: $d_{trans} = \frac{\text{Packet Size (bits)}}{\text{Link Bandwidth (bps)}}$
   - Time to push all packet bits onto the link
4. **Propagation Delay ($d_{prop}$)**
   - Formula: $d_{prop} = \frac{\text{Link Length}}{\text{Speed of Light}}$
   - Time for signal to travel through the medium

---

## Internet Structure

![Internet Architecture](assets/internet_structure.jpg)

The Internet is organized hierarchically as a "network of networks":

### Tier-1 ISPs

- Large commercial Internet Service Providers
- Examples: AT&T, Verizon, Level 3, NTT

### Content Provider Networks

- Servers used to distribute content closer to users
- Examples: Google, Amazon, Facebook, Microsoft

### Interconnection

**Internet Exchange Points (IXPs)**
- a specialized data center where different internet infrastructure companies connect their networks to exchange data directly

**Peering Links**

- Settlement-free interconnection between ISPs.

---

## Protocol Stack

The Internet uses a layered protocol stack where each layer has specific responsibilities:

### Protocol Overview

**Application Layer**
- **HTTP**: Get web pages, images, and post data to servers
- Handles URLs, redirects, caching, proxies, cookies

**Transport Layer**
- **TCP**: Byte streams with ordering, delivery confirmation, pacing
- **UDP**: Simple datagram delivery

**Network Layer**
- **IP**: Forward packets across multiple hops
- Makes best-effort attempt to route packets to destination IP address

**Link Layer**

- **Ethernet/WiFi**: Share communication link with multiple local devices

![layers](assets/layers.jpg)

### Transport Layer Protocols

| **Feature** | **TCP (Transmission Control Protocol)** | **UDP (User Datagram Protocol)** |
|------------|----------------------------------------|----------------------------------|
| **Abstraction** | Reliable, bidirectional byte stream ("pipe") | Simple datagram service; like plain IP with ports |
| **Reliability** | Provides delivery confirmation and retransmission | No reliability; applications must handle errors |
| **Flow Control** | Throttles sender to avoid overwhelming receiver | None |
| **Congestion Control** | Adjusts sending rate based on network conditions | None |
| **Connection Setup** | Requires 3-way handshake to establish connection | No setup delay; connectionless |
| **Use Cases** | Web browsing (HTTP), email (SMTP), file transfer (FTP) | VoIP (SIP/RTP), DNS, streaming, gaming |
| **Overhead** | Higher due to reliability mechanisms | Minimal overhead |
