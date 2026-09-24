# Day 5: IP Addressing Fundamentals & Binary Mathematics

## 1. What is an IP Address?
* **Definition:** An IP (Internet Protocol) address is a unique numerical identifier assigned to each device connected to a computer network to enable device identification and communication.
* **Governing Body:** Managed globally by **IANA** (Internet Assigned Numbers Authority).
* **Versions:**
  * **IPv4** (32-bit address space)
  * **IPv6** (128-bit address space)

---

## 2. IPv4 Characteristics
* **Bit Length:** 32-bit binary number.
* **Structure:** Divided into **4 octets** separated by dots (`.`) (Dotted-decimal notation).
* **Octet Size:** Each octet contains **8 bits** ($4 \times 8\text{ bits} = 32\text{ bits}$).
* **Conversion:** Displayed in human-readable decimal form converted from binary.

---

## 3. Network Environments & Special Addresses

### A. Network Environments
1. **Public Network:** Uses **Public IP Addresses** (routable over the internet).
2. **Private Network:** Uses **Private IP Addresses** (used within local networks / LANs; non-routable over the internet).

### B. Network & Broadcast Addresses
* **Network Address:** The very first IP address in a network subnet range.
  * *Default:* Starts with $0$ in the host portion.
  * *Purpose:* Identifies the network itself. Cannot be assigned to a host device.
* **Broadcast Address:** The very last IP address in a network subnet range.
  * *Default:* Ends with $255$ in the host portion.
  * *Purpose:* Sends traffic to all devices on the network. Cannot be assigned to a host device.
* **Note:** Both Network and Broadcast addresses are **invalid host IPs**, but they define the boundary range of your network.

### C. Loopback Adapter Address (`127.0.0.1`)
* **Loopback Address:** A reserved logical IP address used by a host to test its own network stack and NIC hardware functionality.
* **Characteristics:**
  * Primary loopback IP: `127.0.0.1`.
  * Used for internal diagnostics and link testing.
  * Any IP address on network devices (Routers/Switches) can be configured as a loopback interface for testing and management.

---

## 4. Classes of IPv4 Addresses ($0 \text{ to } 255$)
The class of an IP address is determined by the **First Octet**:

| Class | First Octet Range | Usage / Type | Description |
| :---: | :---: | :--- | :--- |
| **Class A** | $0 \text{ -- } 126$ | **Unicast Address** | Designed for very large networks. |
| **Class B** | $128 \text{ -- } 191$ | **Unicast Address** | Designed for medium-sized networks. |
| **Class C** | $192 \text{ -- } 223$ | **Unicast Address** | Designed for small networks (LANs). |
| **Class D** | $224 \text{ -- } 239$ | **Multicast Address** | Used by routing protocols (e.g., OSPF, EIGRP). |
| **Class E** | $240 \text{ -- } 255$ | **Reserved (Experimental)** | Reserved for Research & Development (R&D). |

* **Note:** Range `127.x.x.x` is missing from Class A because it is reserved for loopback testing.

---

## 5. Binary to Decimal Conversion Mathematics

An 8-bit octet consists of binary positions with values based on powers of $2$ ($2^n$):

$$\text{Bit Position Values}: 2^7, 2^6, 2^5, 2^4, 2^3, 2^2, 2^1, 2^0$$

### Octet Bit Position Values:
* $2^0 \times 1 = 1$
* $2^1 \times 1 = 2$
* $2^2 \times 1 = 4$
* $2^3 \times 1 = 8$
* $2^4 \times 1 = 16$
* $2^5 \times 1 = 32$
* $2^6 \times 1 = 64$
* $2^7 \times 1 = 128$

$$\text{Total Sum when all 8 bits are set to } 1 \text{ (11111111)} = 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255$$

### Octet Range Extremes
* **All Bits 0 (`00000000`):** Decimal Value = $0$
* **All Bits 1 (`11111111`):** Decimal Value = $255$

Full IPv4 Bounds in Binary vs Decimal:
* **Minimum Value:** `00000000.00000000.00000000.00000000` $\rightarrow$ `0.0.0.0`
* **Maximum Value:** `11111111.11111111.11111111.11111111` $\rightarrow$ `255.255.255.255`

---

### Conversion Example: Convert Binary `11000000` to Decimal

1. Write positional values:
   $$\begin{array}{|c|c|c|c|c|c|c|c|}
   \hline
   128 & 64 & 32 & 16 & 8 & 4 & 2 & 1 \\
   \hline
   1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \\
   \hline
   \end{array}$$

2. Add values where bit is `1`:
   $$128 + 64 = 192$$

3. Result: Binary `11000000` = Decimal `192` (First octet of Class C).
