# Day 4: Network Models, Protocols & Port Numbers

## 1. Network Layer Models Comparison

| OSI Reference Model (7 Layers) | TCP/IP Stack Model (5 Layers) | DOD Model (4 Layers) |
| :--- | :--- | :--- |
| **7. Application** | Application | Application / Process |
| **6. Presentation** | | |
| **5. Session** | | |
| **4. Transport** | Transport | Host-To-Host Layer |
| **3. Network** | Network | Internet Layer |
| **2. Data Link** | Data Link | Network Access Layer |
| **1. Physical** | Physical | |

---

## 2. Protocols Overview & Categorization

* **Protocol:** A defined set of rules and regulations governing network communications.

### Protocol Classifications
1. **Routed Protocol:** Pre-defined protocols used to carry user data traffic across a network (e.g., IP, TCP, UDP). They cannot be modified/routed directly by administrators.
2. **Routing Protocol:** Configured by network administrators to dynamically discover paths, build routing tables, and forward traffic (e.g., OSPF, EIGRP, BGP).

---

## 3. Layer-Wise Breakdown of Routed Protocols

### A. Application Layer Protocols
* Manages network services direct to end-user software applications.
* **Key Protocols & Ports (Managed by IANA - Total Range: 0 to 65,535 | Well-Known Ports: 0 to 1023):**
  * **FTP:** Port `21` (Control), Port `20` (Data)
  * **SSH:** Port `22` *(Secure Remote Access)*
  * **Telnet:** Port `23` *(Unencrypted Remote Access)*
  * **SMTP:** Port `25` *(Mail Transfer)*
  * **DNS:** Port `53` *(Resolves IP $\leftrightarrow$ Name)*
  * **DHCP:** Ports `67` (Server), `68` (Client)
  * **TFTP:** Port `69`
  * **HTTP:** Port `80`
  * **HTTPS:** Port `443`
  * **POP3:** Port `110`
  * **IMAP:** Port `143`

---

### B. Transport Layer Protocols

#### 1. TCP (Transmission Control Protocol) — Protocol Number: `6`
* **Characteristics:**
  * **Reliable Protocol:** Guarantees delivery via acknowledgments.
  * **Connection-Oriented:** Establishes a session before sending data.
  * **Supports Acknowledgments:** Receiver confirms packet delivery.
  * **Supports Unicast Communication:** One-to-One transmission.
  * **Establishes connection via 3-Way Handshake.**
* **Example:** Web browsing (HTTPS), file transfer (FTP), and email sending (SMTP) where no data loss can be tolerated.

#### 2. UDP (User Datagram Protocol) — Protocol Number: `17`
* **Characteristics:**
  * **Unreliable Protocol:** Does not guarantee delivery.
  * **Connectionless Protocol:** Sends data immediately without establishing a link first.
  * **No Acknowledgments:** Does not wait for receiver confirmation.
  * **Supports Broadcast & Multicast Communication:** One-to-Many / One-to-All.
  * **Faster than TCP:** Lower overhead due to lack of error checking.
* **Example:** Live video streaming, VoIP calls, and online gaming where speed matters more than minor packet loss.

---

### C. Network Layer Protocols

* **IP (Internet Protocol):** Creates logical IP addresses for device-to-device communication across networks.
  * *Example:* Assigning `192.168.1.10` to a host so it can reach `192.168.1.1`.
* **ICMP (Internet Control Message Protocol):** Used to test and verify connectivity between source and destination devices.
  * *Example:* Running a `ping 8.8.8.8` or `traceroute` command in the terminal.
* **ARP (Address Resolution Protocol):** Resolves a known IP address to an unknown MAC address.
  * *Example:* A PC knows a printer's IP (`192.168.1.50`), and uses ARP to find its hardware MAC address before sending a print job.

---

### D. Data Link Layer Protocols (WAN Protocols)
Operate at Layer 2 to format data into frames for transmission over Wide Area Network links:
* **PPP (Point-to-Point Protocol)**
* **HDLC (High-Level Data Link Control)**
* **Frame-Relay**
* **ATM (Asynchronous Transfer Mode)**

---

### E. Physical Layer Protocols (Standards)
Operate at Layer 1 defining physical signaling and wireless transmission standard specifications:
* **802.1:** Bridging and LAN/MAN Management standards.
* **802.11:** Wi-Fi Wireless LAN networking standards.

---

## 4. Transmission & Configuration Mechanisms

### A. TCP 3-Way Handshake Process
Used by TCP to establish a reliable connection between a Source and Destination host before transferring data.

```
Source                                        Destination
  |                                                |
  | -------------- 1. SYN -----------------------> |  (Synchronize request)
  |                                                |
  | <------------- 2. SYN / ACK ------------------ |  (Acknowledge & Sync back)
  |                                                |
  | -------------- 3. ACK -----------------------> |  (Final Acknowledge)
  |                                                |
  <================ CONNECTION ESTABLISHED ========>
```

#### Steps:
1. **SYN (Synchronize):** Source sends a connection request with an initial sequence number.
2. **SYN/ACK (Synchronize-Acknowledgment):** Destination acknowledges the request and sends back its own synchronization request.
3. **ACK (Acknowledgment):** Source acknowledges the destination's response. Connection is now established.

* **Example:** When you open `https://google.com`, your web browser initiates a 3-way handshake with Google's web server before loading any webpage content.

---

### B. DHCP (Dynamic Host Configuration Protocol) & DORA Process
Automatically assigns dynamic IP addresses and network configurations to host devices joining the network.
* **Server Port:** `67`
* **Client Port:** `68`

```
  Client (Port 68)                               Server (Port 67)
[Unassigned Host]                              [IP Address Pool]
        |                                              |
        | ----------------- 1. DISCOVER -------------> | (Broadcast: Looking for DHCP Server)
        |                                              |
        | <---------------- 2. OFFER ----------------- | (Unicast/Broadcast: Here is an available IP)
        |                                              |
        | ----------------- 3. REQUEST --------------> | (Broadcast: I want to use this IP)
        |                                              |
        | <---------------- 4. ACK / NACK ------------ | (Unicast: Confirmed / Denied)
```

#### The DORA Process:
1. **D - Discover:** Client broadcasts a message to find an available DHCP server on the network.
2. **O - Offer:** DHCP Server responds with an available IP address offer from its IP pool.
3. **R - Request:** Client requests to accept and lease the offered IP address.
4. **A - Acknowledgment (ACK / NACK):** 
   * **ACK (Positive Acknowledgment):** Server confirms the IP lease to the client.
   * **NACK (Negative Acknowledgment):** Server denies the request (e.g., if the offered IP was taken by another device in the interim).

* **Example:** When you turn on Wi-Fi on your smartphone, it broadcasts a **DHCP Discover** request. The router responds with an **Offer** (e.g., `192.168.1.15`), your phone sends a **Request** for it, and the router replies with an **ACK**, completing connection setup.

---

## 5. Network Traffic Analogy (Traffic Light Rules)
* **1 - Red:** Stop Traffic
* **2 - Yellow:** Go Slow / Prepare to Stop
* **3 - Green:** Go Traffic
