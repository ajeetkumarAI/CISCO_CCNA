# Day 2: Network Devices & Transmission Modes

## 1. Network Devices Overview

### 1. Repeater
* **Function:** Regenerates incoming signals to eliminate attenuation and extend transmission distance over copper or fiber media.

### 2. Amplifier
* **Function:** Converts weak analog/digital signals into stronger signals for long-distance transport.

### 3. Hub
* **Function:** Legacy layer-1 device that connects multiple physical host cables together.
* **Key Characteristics:**
  * Provides physical interfaces for user host connectivity.
  * Uses **Broadcasting** for all outbound communications.
  * Operates with **1 Collision Domain** and **1 Broadcast Domain** across all ports.
  * Supports **Half-Duplex** transmission only.
  * **Port Capacity:** Minimum 2 ports to a maximum of 24 ports.
* **Terminology:**
  * **BD (Broadcast Domain):** A group of network users/devices receiving broadcast frames.
  * **CD (Collision Domain):** A network segment where packet collisions can occur during simultaneous transmissions.

### 4. Switch
* **Function:** An intelligent layer-2 network device that forwards data frames based on hardware addresses.
* **Key Characteristics:**
  * Uses **MAC Addresses** for frame forwarding.
  * Stores MAC addresses in hardware memory called a **CAM Table** (Content Addressable Memory / Switching Table).
  * Provides **Multiple Collision Domains** (1 per port) and **1 Broadcast Domain** (default per VLAN).
  * Supports **Full-Duplex** transmission.
  * Traffic Forwarding Types:
    * **Flooding:** Broadcasts traffic when destination MAC is unknown (and updates LRN).
    * **Unicasting:** One-to-One direct communication.
    * **Multicasting:** One-to-Many group communication.
    * **Broadcasting:** One-to-All communication (performed once when MAC is unknown).
* **Port Capacity:**
  * **Unmanaged Switch (NMS):** 8, 24, 48, to 96 ports.
  * **Managed Switch (MS):** More than 96 ports (modular switches).

### 5. Router
* **Function:** An intelligent layer-3 device used to interconnect different logical IP networks and determine optimal pathing.
* **Key Characteristics:**
  * Finds the best path to a destination network.
  * Stores path routing decisions in system memory within a **Routing Table**.
  * Uses **IP Addresses** to forward network layer traffic.
  * Breaks up broadcast domains (each router interface is its own broadcast domain).

---

## 2. Transmission Modes (Duplex Modes)
* **Half-Duplex:** Two-way transmission, but devices can only send or receive one at a time (e.g., Hubs, Walkie-Talkies).
* **Full-Duplex:** Simultaneous two-way transmission and reception at any given time (e.g., Switches, Telephones).
* **Simplex:** One-way unidirectional transmission only (e.g., Radio broadcast, Keyboard to PC).
