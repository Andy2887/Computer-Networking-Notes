# Network Foundations

## Introduction

### What is the Internet?

The Internet is a **"network of networks"** consisting of billions of computing devices (hosts/end systems) interconnected globally.

**Internet vs. The Web:**
- **Internet**: The underlying hardware network infrastructure
- **Web**: An application layer service of interconnected hypertext pages using the HTTP protocol

## Internet Architecture

### Network Edge

The **network edge** includes all hosts (clients and servers) that connect to the Internet:
- End-user devices: smartphones, laptops, IoT devices
- Servers: web servers, application servers, databases
- Typically have a **single link** to the Internet

### Network Core

The **network core** is a mesh of interconnected routers that relay packets between different parts of the Internet:
- Provides the backbone for data transmission
- Routes packets across multiple networks
- Handles traffic between edge networks

### Access Networks

Access networks connect end users to the Internet:

| **Type** | **Description** | **Characteristics** |
|----------|----------------|---------------------|
| **DSL (Digital Subscriber Line)** | Uses existing telephone lines | Data and voice transmitted at different frequencies |
| **Cable** | Uses coaxial cable distribution networks | Originally designed for TV; shared among neighborhoods |
| **Wireless** | Radio-based connectivity | WiFi (short range), Cellular (wide area), Satellite |

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
- Operates at the data link layer
- Forwards data between devices on the same network
- Examples: Ethernet switches, network switches

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

## Switching Methods

### Circuit Switching vs. Packet Switching

| **Feature** | **Circuit Switching** | **Packet Switching** |
|------------|----------------------|---------------------|
| **Method** | Dedicated end-to-end path established for each call | Data sent in small chunks (packets) |
| **Efficiency** | Can be wasteful during silence; "all or nothing" | Highly efficient for "bursty" traffic |
| **Performance** | Performance is guaranteed | "Best effort" delivery; no guarantees |
| **Usage** | Traditional telephone networks (PSTN) | Modern Internet |
| **Resource Allocation** | Resources reserved for duration of connection | Resources shared dynamically |

## Network Performance

### Four Sources of Nodal Delay

Total nodal delay: $d_{nodal} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$

1. **Processing Delay ($d_{proc}$)**
   - Checking for bit errors
   - Determining output link
   - Typically microseconds or less

2. **Queueing Delay ($d_{queue}$)**
   - Time waiting at output link queue
   - Depends on congestion level
   - If the finite-sized queue is full, **packets are dropped**

3. **Transmission Delay ($d_{trans}$)**
   - Formula: $d_{trans} = \frac{\text{Packet Size (bits)}}{\text{Link Bandwidth (bps)}}$
   - Time to push all packet bits onto the link

4. **Propagation Delay ($d_{prop}$)**
   - Formula: $d_{prop} = \frac{\text{Link Length}}{\text{Speed of Light}}$
   - Time for signal to travel through the medium

### Performance Metrics

**Throughput (Bandwidth)**
- The rate of data transfer
- Measured in bits per second (bps)
- Can be different from link capacity due to congestion

**Latency**
- The time it takes for data to reach its destination
- One-way measurement

**Round Trip Time (RTT)**
- The time it takes to get a response from a packet
- Includes both transmission and acknowledgment

**Performance Distribution**
- Often analyzed using **Cumulative Distribution Function (CDF)**
- Shows the full range of measurements
- Common metrics: median, 95th percentile (p95)

## Internet Structure

![Internet Architecture](assets/internet_structure.jpg)

The Internet is organized hierarchically as a "network of networks":

### Tier-1 ISPs

- Large commercial Internet Service Providers
- National and international coverage
- Form the backbone of the Internet
- Examples: AT&T, Verizon, Level 3, NTT

### Content Provider Networks

- Private networks operated by large content providers
- Connect distributed data centers directly to various ISPs
- Bypass traditional Internet routing to speed up access
- Examples: Google, Amazon, Facebook, Microsoft

### Interconnection

**Internet Exchange Points (IXPs)**
- Physical locations where multiple networks interconnect
- Allow direct exchange of traffic between networks
- Reduce latency and costs

**Peering**
- Settlement-free interconnection between networks
- Networks exchange traffic without payment
- Mutual benefit arrangement

## Protocol Stack

The Internet uses a layered protocol stack where each layer has specific responsibilities:

### Protocol Overview

**Application Layer**
- **HTTP**: Get web pages, images, and post data to servers
- Handles URLs, redirects, caching, proxies, cookies

**Transport Layer**
- **TCP**: Byte streams with ordering, delivery confirmation, pacing
- **UDP**: Simple datagram delivery
- Provides file-like interface to network connections

**Network Layer**
- **IP**: Forward packets across multiple hops
- Makes best-effort attempt to route packets to destination IP address

**Link Layer**
- **Ethernet/WiFi**: Share communication link with multiple local devices
- Handles physical transmission over local networks

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

**When to Use TCP:**
- Reliable data delivery is critical
- Order of data matters
- File transfers, web pages, email

**When to Use UDP:**
- Speed is more important than reliability
- Real-time applications can tolerate some packet loss
- VoIP, video streaming, online gaming, DNS queries
