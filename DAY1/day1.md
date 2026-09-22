# Day 1: Introduction to Cisco CCNA & Physical Layer Basics

## 1. Cisco CCNA Overview
* **CCNA (Cisco Certified Network Associate)** is a foundational certification validating knowledge in network fundamentals, network access, IP connectivity, IP services, security fundamentals, and automation.
* **Core Objective:** To facilitate seamless, reliable, and secure communication across local area networks (LANs) and wide area networks (WANs) using Cisco routers, switches, and standard network protocols.

---

## 2. Fundamental Network Components
* **NIC (Network Interface Card):** Used for wired communication interfaces on host devices.
* **WIC (WAN Interface Card):** Used for wireless or WAN interface communication connections.
* **Driver:** Operating system software interface enabling communication between computer hardware and the network interface card.

---

## 3. Network Addressing
* **IP Address (Logical Address):** Software-assigned address used for routing network layer traffic.
  * **Nature:** Temporary / Dynamic.
* **MAC Address (Physical Address):** Hardware address assigned to the network interface.
  * **Nature:** Permanent / Burned-in Address (BIA).

---

## 4. Physical Layer Media & Connectors

### Transmission Cables
* **Twisted Pair Cables:** 
  * **UTP** (Unshielded Twisted Pair)
  * **STP** (Shielded Twisted Pair)
* **Co-axial Cables:**
  * **Thinnet** (10Base2)
  * **Thicknet** (10Base5)
* **Fibre Optic Cables:**
  * **Single-mode Fibre** (Long-range, laser light source)
  * **Multi-mode Fibre** (Short-range, LED light source)

### Network Connectors
* **RJ45 & RJ11:** Used for Twisted Pair cables.
  * **RJ45:** 8-pin connector (Pins 1, 2, 3, 6 actively used for data transmission/reception in Fast Ethernet).
* **BNC Connector:** Used for Co-axial cables.
* **SC & ST Connectors:** Used for Fibre Optic connections.

---

## 5. MAC Address Deep Dive
* **Definition:** Also known as Physical Address, Machine Address, or Permanent Address.
* **Structure:**
  * **48-bit binary number** represented in **12 Hexadecimal characters** (1 Hex character = 4 bits).
  * Uses Hexadecimal numbering ($0\text{--}9$ and $\text{A}\text{--}\text{F}$).

### MAC Address Formatting Example: `B3:BC:94:71:C5:75`
* **Vendor ID (OUI - Organizationally Unique Identifier):** The first 3 bytes / 6 hex digits (`B3:BC:94`). Identifies the manufacturer.
* **Serial Number (NIC Specific):** The last 3 bytes / 6 hex digits (`71:C5:75`). Unique interface identifier assigned by the manufacturer.

### Binary to Hexadecimal Reference Chart
| Binary | Decimal / Hexadecimal | Binary | Decimal / Hexadecimal |
| :---: | :---: | :---: | :---: |
| `0000` | 0 | `1000` | 8 |
| `0001` | 1 | `1001` | 9 |
| `0010` | 2 | `1010` | 10 (A) |
| `0011` | 3 | `1011` | 11 (B) |
| `0100` | 4 | `1100` | 12 (C) |
| `0101` | 5 | `1101` | 13 (D) |
| `0110` | 6 | `1110` | 14 (E) |
| `0111` | 7 | `1111` | 15 (F) |
