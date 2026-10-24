# Networking-note-cybersec101-
Here are the notes after I finished tryhackme networking module
# Comprehensive Networking & Security Notes

## Network Models

### The 7-Layer OSI Model

1. **Physical Layer** — The physical hardware and cables (e.g., running underground).
2. **Data Link Layer** — Defines protocols for node-to-node/local delivery and utilizes MAC addresses.
3. **Network Layer** — Defines logical addressing, routing, and packet forwarding (e.g., IP, ICMP).
4. **Transport Layer** — Handles end-to-end communication protocols (e.g., TCP, UDP).
5. **Session Layer** — Establishes, maintains, and synchronizes communication between applications on different hosts (e.g., NFS, RPC).
6. **Presentation Layer** — Handles data encoding, compression, and encryption (e.g., Unicode, MIME, JPEG).
7. **Application Layer** — Provides services and interfaces directly to software applications (e.g., HTTP, FTP, DNS).

### The 4-Layer TCP/IP Model

1. **Link Layer** — Encompasses the OSI Data Link and Physical layers.
2. **Internet Layer** — Matches the OSI Network layer.
3. **Transport Layer** — Matches the OSI Transport layer.
4. **Application Layer** — Encompasses the OSI Application, Presentation, and Session layers.

---

## Data Transmission & Encapsulation

### TCP vs. UDP

| Protocol | Connection Type | Characteristics | Use Cases |
| --- | --- | --- | --- |
| **TCP** | Connection-Oriented | Uses a 3-way handshake; uses flags (`SYN`, `SYN-ACK`, `ACK`). | Web browsing, email, file transfers. |
| **UDP** | Connectionless | Fast, no guaranteed delivery or sequencing. | Video games, live streaming. |

### The Encapsulation Process

When sending data over a network, it gets wrapped in successive layers of headers:

1. **Application Data:**
The raw data payload to be sent.


2. **Transport Segment:**
Adds the proper header info to create a **TCP segment** or **UDP datagram**.


3. **Network Packet:**
Adds the **IP header** for logical routing.


4. **Data Link Frame:**
Adds the **MAC header and trailer** to create a frame ready for physical transmission.


---

## Core Networking Protocols

* **DHCP (Dynamic Host Configuration Protocol):** Automates the configuration of IP addresses, subnet masks, gateways, and DNS.
* Relies on UDP (Server listens on `67`, Client sends from `68`).
* Follows a 4-step process: **D**iscovery, **O**ffer, **R**equest, **A**cknowledge (DORA).
* *Tshark command:* `tshark -r DHCP-G5000.pcap -n`


* **ARP (Address Resolution Protocol):** Resolves IP addresses to MAC addresses.
* Encapsulated directly inside an Ethernet frame (not UDP/IP).
* Destination MAC for an ARP request is a broadcast: `ff:ff:ff:ff:ff:ff`.


* **ICMP (Internet Control Message Protocol):** Used for network diagnosis and error reporting.
* Tools: `ping` (tests reachability and RTT) and `traceroute` / `tracert` (maps router hops).
* *Example:* `ping 192.168.11.1 -c 4` (stops after 4 packets).


* **Routing:** Algorithms used to map the best path for packets (OSPF, EIGRP, BGP, RIP).
* **NAT (Network Address Translation):** Translates one public IP address into multiple private IP addresses, including translating port numbers.

---

## Application Layer Protocols

### Domain Name System (DNS)

Operates on **Port 53** (UDP by default, TCP fallback). Maps domain names to IP addresses.

* **A Record:** IPv4 addresses.
* **AAAA Record:** IPv6 addresses.
* **CNAME Record:** Maps a domain alias to a canonical domain (e.g., `[www.example.com](https://www.example.com)` to `example.com`).
* **MX Record:** Mail server routing.
* *Command:* `nslookup [www.example.com](https://www.example.com)` / `whois twitter.com` (shows domain registry info).

### Web & File Transfer

* **HTTP / HTTPS:** Retrieves and submits web data using methods like `GET`, `POST`, `PUT`, `DELETE`.
* HTTP uses **Port 80**. HTTPS uses **Port 443** (HTTP over TLS).


* **FTP (File Transfer Protocol):** Listens on **Port 21**. Used to transfer files.
* *Core Commands:* `USER`, `PASS`, `RETR` (download), `STOR` (upload).


* **Telnet:** Connects to remote systems to issue raw text commands. *(Insecure, use SSH instead).*

### Email Protocols

* **SMTP (Sending):** Port **25**. Used to push mail to a server.
* *Commands:* `HELO`/`EHLO`, `MAIL FROM`, `RCPT TO`, `DATA`. (A `.` on a single line ends the message).


* **POP3 (Receiving):** Port **110**. Downloads mail to a client, usually removing it from the server.
* *Commands:* `USER`, `PASS`, `STAT`, `LIST`, `RETR`, `DELE`, `QUIT`.


* **IMAP (Synchronizing):** Port **143**. Keeps mail on the server and syncs across devices.
* *Commands:* `LOGIN`, `SELECT`, `FETCH`, `MOVE`, `COPY`, `LOGOUT`.



---

## Secure Protocols & Encryption

* **TLS (Transport Layer Security):** Operates at the transport layer. Servers require a certificate signed by a Certificate Authority (CA) to prove their identity.
* **HTTPS:** Uses a 3-step process: TCP Handshake → TLS Session Establishment → HTTP Communication.
* **Secure Email:** **SMTPS** (Port 465/587), **POP3S** (Port 995), **IMAPS** (Port 993).
* **SSH (Secure Shell):** Port **22**. An encrypted replacement for Telnet.
* Provides secure authentication (keys, 2FA), confidentiality, integrity, and tunneling (VPN-like routing).
* Supports X11 Forwarding (`ssh 192.168.124.148 -X`) for running remote graphical apps locally.


* **SFTP / FTPS:** Secure file transfer. SFTP uses SSH (**Port 22**). FTPS uses **Port 990** and requires certificates.
* **VPN (Virtual Private Network):** Securely connects disparate networks across geological locations, preventing data alteration or eavesdropping.

---

## Packet Analysis Tools

### Wireshark & IP Lookups

* **IP Lookup Commands:** `ipconfig` (Windows), `ifconfig` or `ip a s` (Linux).
* **Wireshark:** A GUI traffic analyzer used for troubleshooting network loads, detecting security anomalies (rogue hosts), and investigating payload data.

### Tcpdump Flag Glossary

*Command syntax example:* `tcpdump -i eth0 -c 50 -v`

| Flag / Syntax | Function |
| --- | --- |
| **`-i [interface]`** | Captures packets on a specific interface (e.g., `eth0`, `any`). |
| **`-w [file]`** | Writes captured packets to a file (prevents screen scrolling). |
| **`-r [file]`** | Reads packets from a saved capture file. |
| **`-c [count]`** | Stops capturing after the specified number of packets. |
| **`-n` / `-nn**` | Prevents DNS resolution (`-n`) and port name resolution (`-nn`). |
| **`-v` / `-vv` / `-vvv**` | Increases verbosity (shows TTL, lengths, options, etc.). |
| **`host [IP]`** | Limits capture to traffic involving a specific host. |
| **`src host` / `dst host**` | Limits capture to a specific source or destination IP. |
| **`port [num]`** | Limits capture to a specific port. |
| **`greater` / `less**` | Filters packets by length threshold. |
| **`proto[expr:size]`** | Advanced byte-offset filtering (e.g., `ether[0] & 1 != 0` for multicast). |
| **`tcp[tcpflags]`** | Filters by TCP flag (e.g., `tcp[tcpflags] == tcp-syn`). |
| **`-q`** | Quick output (brief information). |
| **`-e`** | Prints the link-level (MAC) header. |
| **`-A` / `-xx` / `-X**` | Shows packet data in ASCII (`-A`), Hex (`-xx`), or both (`-X`). |

---

## Network Scanning: Nmap

*Target selection:* Can use ranges (`192.168.0.1-10`) or CIDR notation (`192.168.0.0/24`).

### Nmap Flag Glossary

| Flag | Function |
| --- | --- |
| **`-sn`** | **Ping Scan:** Discovers online hosts without port scanning. |
| **`-sL`** | **List Scan:** Lists targets without actually scanning them. |
| **`-sT`** | **TCP Connect Scan:** Performs full 3-way handshakes to test ports. |
| **`-sS`** | **SYN Scan:** (Stealth) Sends SYN packets without completing the handshake. |
| **`-sU`** | **UDP Scan:** Scans UDP ports. |
| **`-F`** | **Fast Mode:** Scans only the top 100 common ports (instead of 1000). |
| **`-p [range]`** | **Port Range:** Specifies exact ports (e.g., `-p 10-1024`, `-p-25`). |
| **`-O`** | **OS Detection:** Attempts to identify the target's operating system. |
| **`-sV`** | **Version Detection:** Probes open ports to determine service and version info. |
| **`-A`** | **Aggressive:** Enables OS detection, version scanning, script scanning, and traceroute. |
| **`-Pn`** | **No Ping:** Treats all hosts as online (useful if ICMP is blocked). |
| **`-T0` to `-T5**` | **Timing:** Controls scan speed (`T0` is slowest/stealthiest, `T5` is fastest/loudest). |
| **`--min-parallelism`** | Sets minimum/maximum number of parallel probe operations. |
| **`--min-rate`** | Sets minimum/maximum packet transmission rate. |
| **`--host-timeout`** | Maximum time willing to wait for a single host to complete scanning. |
| **`-v` / `-d**` | Increases verbosity (`-v`) or debugging output (`-d` up to `-d9`). |

**Output Formats:**

* `-oN` (Normal), `-oX` (XML), `-oG` (Grep-able), `-oA` (Outputs all major formats).