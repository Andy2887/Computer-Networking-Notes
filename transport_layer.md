# Transport Layer

---

## UDP

The **User Datagram Protocol (UDP)** is the simplest transport protocol. It is essentially IP with two small additions:

1. **Port Numbers:** To tell the computer which application the packet belongs to.
2. **Checksums:** A mathematical summary of the data used to detect corruption.

> **Crucial Note:** UDP is **unreliable**. If a packet is lost, UDP doesn't care. If a bit is flipped, the checksum detects it, and the receiver simply **drops** the packet. It does not try to fix it.

![](assets/udp_packet.jpg)

---

## Checksum

1. Break the data into a sequence of 16-bit integers
2. Add the integers
3. Wrap the carry-out bits to the least-significant position
4. Invert the result.
5. Include the Checksum into the package.

![](assets/checksum.jpg)

---

## TCP

To make a connection "reliable" (like TCP), we have to solve the problems of the underlying Internet: **packet loss, corruption, and reordering.**

### Stop-and-Wait

The simplest computer solution is **Stop-and-Wait**:

1. Sender sends a packet.
2. Sender waits for an ACK.
3. If a timer expires (timeout) before the ACK arrives, the sender retransmits.

**The Problem?** It’s incredibly slow. If you’re sending data from New Jersey to California, the "dead time" spent waiting for ACKs means you’d only use a tiny fraction of your link's actual speed.

### Pipelining

Instead of sending one packet and waiting, we send a "window" of multiple packets at once.

![](assets/pipeline.jpg)

**Sequence numbers:** sequence numbers identify each data segment with an increasing integer. It allows receiver to correctly order and reassemble the received data.

ACKs also must carry sequence numbers. Sender has multiple data segments in flight, so the ACK must specify which of the several sequence numbers was received.

#### Strategy 1: Go Back N

Window size is N, sender can have up to N packets in flight.

Receiver sends cumulative ACK: “I got everything up to seq. number x”

If sender does not get an ACK after some timeout interval, resend all packets starting from packet after the last ACK’ed packet.

![](assets/go-back-n.jpg)

#### Strategy 2: Selective Repeat

In Selective Repeat, both the sender and receiver maintain a window of size $N$.

##### 1. The Sender's Logic

- **The Window:** The sender can transmit up to $N$ packets without waiting for an ACK.
- **Individual ACKs:** Unlike Go-Back-N, the sender tracks ACKs for **each packet individually**.
- **The Slide:** The sender's window "base" only moves forward when the **oldest unacknowledged packet** is finally ACKed. If the sender receives ACKs for packets further ahead in the window, those packets are marked as "received," but the window stays put until the "hole" at the base is filled.

##### 2. The Receiver's Logic

- **The Window:** The receiver also has a window of size $N$, allowing it to accept packets that arrive out of order.
- **Buffering:** If the receiver gets a packet that is within its window but is *not* the next one in sequence (the base), it **buffers** it.
- **The Slide:** The receiver's window "base" only moves forward when the **oldest missing packet** arrives. At that moment, the window slides forward as far as possible, delivering all contiguous, buffered packets to the application layer.
