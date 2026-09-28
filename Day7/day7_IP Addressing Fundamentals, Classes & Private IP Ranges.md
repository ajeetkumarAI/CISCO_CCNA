# IP Addressing Fundamentals, Classes & Private IP Ranges

## 1. What is an IP Address?

* **Definition:** An IP (Internet Protocol) address is a unique numerical identifier assigned to each device connected to a computer network to enable device identification and communication.

* **Governing Body:** Managed globally by **IANA** (Internet Assigned Numbers Authority).

* **Versions:**

  * **IPv4** (32-bit address space)

  * **IPv6** (128-bit address space)

## 2. IPv4 Structure & Characteristics

* **Bit Length:** 32-bit binary number.

* **Structure:** Divided into **4 octets** separated by dots (`.`) (Dotted-decimal notation).

* **Octet Size:** Each octet contains **8 bits** ($4 \times 8\text{ bits} = 32\text{ bits}$).

* **Conversion:** Displayed in human-readable decimal form converted from binary.

### Binary to Decimal Positional Values

An 8-bit octet consists of binary positions with values based on powers of $2$ ($2^n$):

$$
\text{Bit Position Values}: 2^7, 2^6, 2^5, 2^4, 2^3, 2^2, 2^1, 2^0
$$

* **Positional Values:** $128, 64, 32, 16, 8, 4, 2, 1$

* **Maximum Octet Value:** $128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255$

---

## 3. Network ID (N) vs. Host ID (H) Mechanics

Every IPv4 address consists of two parts:

1. **Network ID ($\text{N}$):** Identifies the specific network segment (Fixed portion). Devices in the same subnet share the same Network ID.

2. **Host ID ($\text{H}$):** Identifies a specific device/interface on that network (Variable portion).

### Network Structure Table by Class

| Class | Network Portion ($\text{N}$) | Host Portion ($\text{H}$) | Format | Default Subnet Mask |
| :--- | :--- | :--- | :--- | :--- |
| **Class A** | $1\text{ Octet (8 bits)}$ | $3\text{ Octets (24 bits)}$ | $\text{N.H.H.H}$ | `255.0.0.0` |
| **Class B** | $2\text{ Octets (16 bits)}$ | $2\text{ Octets (16 bits)}$ | $\text{N.N.H.H}$ | `255.255.0.0` |
| **Class C** | $3\text{ Octets (24 bits)}$ | $1\text{ Octet (8 bits)}$ | $\text{N.N.N.H}$ | `255.255.255.0` |

---

## 4. IP Classes & Address Ranges

The class of an IP address is determined by the **First Octet**:

| Class | First Octet Range | Usage / Type | Network/Host Split | Total Usable Hosts per Network |
| :--- | :--- | :--- | :--- | :--- |
| **Class A** | $0 \text{ -- } 126$ | **Unicast Address** | $\text{N.H.H.H}$ | $2^{24} - 2 = 16,777,214$ |
| **Class B** | $128 \text{ -- } 191$ | **Unicast Address** | $\text{N.N.H.H}$ | $2^{16} - 2 = 65,534$ |
| **Class C** | $192 \text{ -- } 223$ | **Unicast Address** | $\text{N.N.N.H}$ | $2^8 - 2 = 254$ |
| **Class D** | $224 \text{ -- } 239$ | **Multicast Address** | N/A | Reserved for Multicast Protocols |
| **Class E** | $240 \text{ -- } 255$ | **Reserved (Experimental)** | N/A | Reserved for R&D |

* **Special Reserved Loopback Range:** `127.0.0.0` to `127.255.255.255` (Primary testing loopback: `127.0.0.1`).

---

## 5. Public vs. Private IP Address Ranges

### A. Network Environments

* **Public IP Address:** Globally unique, assigned by IANA/ISPs, routable on the public Internet.

* **Private IP Address:** Used within internal Local Area Networks (LANs). Non-routable on the Internet (translated via NAT).

---

### B. Detailed Breakdown of Private IP Ranges

#### 1. Class A Private Network

* **Full Class A Range:** `0.0.0.0` to `126.255.255.255`

* **Private Range:** `10.0.0.0` to `10.255.255.255`

* **Number of Private Networks:** $1\text{ Private Network}$ (`10.0.0.0/8`)

* **Usable Host IPs per Network:** $16,777,214$

* **Example Range Sequence:**
  * `10.0.0.0` (Network ID)
  * `10.0.0.1` $\rightarrow$ `10.255.255.254` (Usable Hosts)
  * `10.255.255.255` (Broadcast Address)

---

#### 2. Class B Private Network

* **Full Class B Range:** `128.0.0.0` to `191.255.255.255`

* **Private Range:** `172.16.0.0` to `172.31.255.255`

* **Number of Private Networks:** $16\text{ Private Networks}$ (`172.16.0.0` to `172.31.0.0`)

* **Usable Host IPs per Network:** $65,534$

* **Example Range Sequence:**
  * Network 1: `172.16.0.0` $\rightarrow$ `172.16.255.255`
  * Network 2: `172.17.0.0` $\rightarrow$ `172.17.255.255`
  * Network 3: `172.18.0.0` $\rightarrow$ `172.18.255.255`
  * ...
  * Network 16: `172.31.0.0` $\rightarrow$ `172.31.255.255`

---

#### 3. Class C Private Network

* **Full Class C Range:** `192.0.0.0` to `223.255.255.255`

* **Private Range:** `192.168.0.0` to `192.168.255.255`

* **Number of Private Networks:** $256\text{ Private Networks}$ (`192.168.0.0` to `192.168.255.0`)

* **Usable Host IPs per Network:** $254$

* **Example Range Sequence:**
  * Network 1: `192.168.0.0` $\rightarrow$ `192.168.0.255`
  * Network 2: `192.168.1.0` $\rightarrow$ `192.168.1.255`
  * Network 3: `192.168.2.0` $\rightarrow$ `192.168.2.255`
  * ...
  * Network 255: `192.168.254.0` $\rightarrow$ `192.168.254.255`
  * Network 256: `192.168.255.0` $\rightarrow$ `192.168.255.255`

---

## 6. Subnet Calculations & Special Addresses

### A. Usable Host Calculation Formula

$$
\text{Usable Hosts} = 2^H - 2
$$

Where $H$ is the number of bits in the Host portion ($\text{H}$). The subtraction of $2$ accounts for:
1. **$-1$ Network Address:** First IP in the range ($0$ in host octet).
2. **$-1$ Broadcast Address:** Last IP in the range ($255$ in host octet).

### B. Practical Identification Examples

#### Example 1: Analyze IP `192.168.1.50`
* **First Octet:** `192` $\rightarrow$ Class C
* **Structure:** $\text{N.N.N.H}$ (`192.168.1` is Network ID, `.50` is Host ID)
* **Network Address:** `192.168.1.0`
* **Broadcast Address:** `192.168.1.255`
* **Address Type:** Private Usable Host IP

#### Example 2: Analyze IP `10.50.100.1`
* **First Octet:** `10` $\rightarrow$ Class A
* **Structure:** $\text{N.H.H.H}$ (`10` is Network ID, `.50.100.1` is Host ID)
* **Network Address:** `10.0.0.0`
* **Broadcast Address:** `10.255.255.255`
* **Address Type:** Private Usable Host IP
