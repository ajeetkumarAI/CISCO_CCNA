# Day 4: Network Models, Protocols & Port Numbers

## 1. Network Layer Models Comparison

| OSI Reference Model (7 Layers) | TCP/IP Stack Model (5 Layers) | DOD Model (4 Layers) |
| :--- | :--- | :--- |
| 7. Application | Application | Application / Process |
| 6. Presentation | | |
| 5. Session | | |
| 4. Transport | Transport | Host-To-Host Layer |
| 3. Network | Network | Internet Layer |
| 2. Data Link | Data Link | Network Access Layer |
| 1. Physical | Physical | |

---

## 2. Protocols Overview
* **Protocol:** A defined set of rules and regulations governing network communications.

### Types of Protocols
1. **Routed Protocol:** Pre-defined protocol used to carry user data across a network (e.g., IP, IPv6). Cannot be modified/routed by administrators directly.
2. **Routing Protocol:** Configured by network administrators to dynamic discover paths and build routing tables (e.g., OSPF, EIGRP, BGP).

---

## 3. Port Numbers & Well-Known Application Protocols
* **Port Range:** Managed by **IANA** (Internet Assigned Numbers Authority) spanning from `0` to `65,535`.
* **Well-Known Ports:** Reserved range from `0` to `1023`.

### Common Application Protocols & Port Mapping
* **FTP (File Transfer Protocol):** Port `21` (Control), Port `20` (Data)
* **SSH (Secure Shell):** Port `22` *(Secure Remote Access)*
* **Telnet:** Port `23` *(Unencrypted Remote Access)*
* **SMTP (Simple Mail Transfer Protocol):** Port `25`
* **DNS (Domain Name System):** Port `53` *(Resolves IP to Domain Name & Name to IP)*
* **DHCP (Dynamic Host Configuration Protocol):** Ports `67` (Server), `68` (Client)
* **TFTP (Trivial File Transfer Protocol):** Port `69`
* **HTTP (Hypertext Transfer Protocol):** Port `80`
* **HTTPS (HTTP Secure):** Port `443`
* **POP3 (Post Office Protocol v3):** Port `110`
* **IMAP (Internet Message Access Protocol):** Port `143`

---

## 4. Network Traffic Analogy (Traffic Light Rules)
* **1 - Red:** Stop Traffic
* **2 - Yellow:** Go Slow / Prepare to Stop
* **3 - Green:** Go Traffic
