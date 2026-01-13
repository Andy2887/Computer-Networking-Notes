# Application Layer

---

## Application-layer protocols

**Purpose:** allow apps running on different computers to communicate.

<img src="assets/application.jpg" style="zoom:50%;" />

---

## Architectures

**Client-server Architecture:** client will send request, and server will respond.

**Peer-to-peer Architecture:** all participants have equal responsibilities, and there is no central servers.

<img src="assets/p2p.jpg" style="zoom:50%;" />

---

## Hyper Text Transport Protocol (HTTP)

HTTP was invented for browsers to fetch pages from webservers

**Request:**

- A human-readable header with: URL, method, (plus some optional headers)
- An optional body, storing raw data (bytes).

**Response:**

- A human-readable header with response code, (plus some optional headers)
- An optional body.

### Steps:

1. Client creates TCP socket (a bi-directional pipe of bytes), and server accepts the socket. 
2. Client writes HTTP request to socket; starts listening for response. 
3. Server notices new data on socket and starts reading request data. 
4. Server eventually notices that it has received a full HTTP request. 
5. Server does some work to generate an appropriate response. 
6. Server writes HTTP response to socket. 
7. Client reads and parses response data; stops reading after calculating that the response is complete.

*Other:*

**Cookie**: a small piece of data sent from a web server and stored on the user's computer by the web browser.

Cookies are generally used for **session management:** Keeping you logged in as you move between pages, or remembering what's in your shopping cart.

---

## Simple Mail Transport Protocol (SMTP)

SMTP is a **P2P protocol** used by mail servers to exchange users’ messages.

Mail servers act as clients when sending, and as servers when receiving. Each domain has its own mail server(s).

<img src="assets/STMP.jpg" style="zoom:50%;" />