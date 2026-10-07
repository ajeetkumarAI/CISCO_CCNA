# Cisco Packet Tracer Lab Guide: IP Routing & Multi-Subnet Setup

---

## Module 1: Network Setup & VLSM Subnetting (Mumbai, Delhi & WAN)

### 1. Lab Scenario & Requirements

You need to connect two branch offices—**Mumbai** and **Delhi**—over a WAN link using **Cisco Packet Tracer**.

#### Requirements:
1. **Mumbai LAN (LAN 1):** Needs **40 IP addresses** for host devices.
2. **Delhi LAN (LAN 2):** Needs **20 IP addresses** for host devices.
3. **WAN Link (Serial Link):** Connects Mumbai Router to Delhi Router (**2 IP addresses** needed).
4. **Base IP Address Block:** `192.168.0.0 /24`

---

### 2. VLSM IP Address Calculation Table

Using **Variable Length Subnet Masking (VLSM)**, always allocate IP blocks starting with the largest host requirement first:

| Network Name | Required IPs | Block Size ($2^n$) | CIDR Prefix | Subnet Mask | Network ID (NID) | Usable Host Range | Default Gateway (Router IP) | Broadcast ID (BID) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1. Mumbai LAN** | 40 | 64 ($2^6$) | `/26` | `255.255.255.192` | `192.168.0.0` | `192.168.0.1` – `192.168.0.62` | `192.168.0.1` | `192.168.0.63` |
| **2. Delhi LAN** | 20 | 32 ($2^5$) | `/27` | `255.255.255.224` | `192.168.0.64` | `192.168.0.65` – `192.168.0.94` | `192.168.0.65` | `192.168.0.95` |
| **3. WAN Link** | 2 | 4 ($2^2$) | `/30` | `255.255.255.252` | `192.168.0.96` | `192.168.0.97` – `192.168.0.98` | N/A (Point-to-Point) | `192.168.0.99` |

---

### 3. Physical Devices & Cabling Setup

#### Devices to Drag and Drop in Packet Tracer:
* **2x Routers:** 1941 or 2911 model (Name them **Router_Mumbai** and **Router_Delhi**).
* **2x Switches:** 2960 model (Name them **Switch_Mumbai** and **Switch_Delhi**).
* **4x End Devices (PCs):** PC0, PC1 in Mumbai; PC2, PC3 in Delhi.

> ⚠️ **Important (Adding Serial Interfaces):**
> Cisco 1941 / 2911 routers do not have Serial ports by default.
> 1. Click on the Router $\rightarrow$ **Physical Tab**.
> 2. Turn off the **Power Switch**.
> 3. Drag the **HWIC-2T** module into the empty slot.
> 4. Turn the **Power Switch** back ON.

#### Cabling Connection Guide:

| Source Device & Port | Destination Device & Port | Cable Type | Reason / Rule |
| :--- | :--- | :--- | :--- |
| **PC0 (FastEthernet0)** | **Switch_Mumbai (Fa0/2)** | Straight-Through Cable | Dissimilar Devices |
| **PC1 (FastEthernet0)** | **Switch_Mumbai (Fa0/3)** | Straight-Through Cable | Dissimilar Devices |
| **Switch_Mumbai (Fa0/1)** | **Router_Mumbai (Gig0/0/0)** | Straight-Through Cable | Dissimilar Devices |
| **PC2 (FastEthernet0)** | **Switch_Delhi (Fa0/2)** | Straight-Through Cable | Dissimilar Devices |
| **PC3 (FastEthernet0)** | **Switch_Delhi (Fa0/3)** | Straight-Through Cable | Dissimilar Devices |
| **Switch_Delhi (Fa0/1)** | **Router_Delhi (Gig0/0/0)** | Straight-Through Cable | Dissimilar Devices |
| **Router_Mumbai (Se0/1/0)** | **Router_Delhi (Se0/1/0)** | Serial DCE Cable | Router-to-Router WAN Link |

---

### 4. End-Device (PC) IP & Gateway Configuration

Click on each PC $\rightarrow$ **Desktop Tab** $\rightarrow$ **IP Configuration**:

#### A. Mumbai PCs (Subnet: `192.168.0.0/26`)
* **PC0 Configuration:**
  * **IP Address:** `192.168.0.2`
  * **Subnet Mask:** `255.255.255.192`
  * **Default Gateway:** `192.168.0.1`
* **PC1 Configuration:**
  * **IP Address:** `192.168.0.3`
  * **Subnet Mask:** `255.255.255.192`
  * **Default Gateway:** `192.168.0.1`

#### B. Delhi PCs (Subnet: `192.168.0.64/27`)
* **PC2 Configuration:**
  * **IP Address:** `192.168.0.66`
  * **Subnet Mask:** `255.255.255.224`
  * **Default Gateway:** `192.168.0.65`
* **PC3 Configuration:**
  * **IP Address:** `192.168.0.67`
  * **Subnet Mask:** `255.255.255.224`
  * **Default Gateway:** `192.168.0.65`

---

## Module 2: IP Routing Theory & Concepts

### 1. Routing Hierarchy & Classification

```
                          ┌──────────────────┐
                          │ Routing Protocol │
                          └────────┬─────────┘
                                   │
         ┌─────────────────────────┴─────────────────────────┐
         ▼                                                   ▼
  ┌────────────┐                                   ┌─────────────────┐
  │ IP Routing │ (Manual Configuration)            │ Dynamic Routing │ (Automatic Protocol-based)
  └──────┬─────┘                                   └────────┬────────┘
         │                                                  │
   ┌─────┴──────────────┐                           ┌───────┴──────────────┐
   ▼                    ▼                           ▼                      ▼
┌────────┐         ┌─────────┐                ┌───────────┐          ┌───────────┐
│ Static │         │ Default │                │    IGP    │          │    EGP    │
│ Route  │         │  Route  │                └─────┬─────┘          └─────┬─────┘
└────────┘         └─────────┘                      │                      │
(Known Dest.)     (Unknown Dest.)         ┌─────────┼─────────┐         ┌──┴──┐
                                          ▼         ▼         ▼         ▼     ▼
                                        Distance   Link    Hybrid      BGP   EIGRP
                                         Vector    State  (EIGRP)
                                        (RIP/IGRP) (OSPF)
```

---

### 2. Manual IP Routing Types

#### A. Static Routing
* **Definition:** Routes are entered manually by the network administrator.
* **Use Case:** Small networks with fixed topology, where destination network paths are known.
* **Rule:** Applies **only to indirectly connected networks**.
* **Syntax:**
  ```text
  Router(config)# ip route <Destination_Network_Address> <Destination_Subnet_Mask> <Next_Hop_IP_or_Exit_Interface>
  ```

#### B. Default Routing
* **Definition:** A special form of static route used when the specific destination network address is unknown or when forwarding packets to an ISP / Internet gateway.
* **Use Case:** Stub networks (networks with only a single exit path).
* **Syntax:**
  ```text
  Router(config)# ip route 0.0.0.0 0.0.0.0 <Next_Hop_IP_or_Exit_Interface>
  ```

---

### 3. Dynamic Routing Protocols Overview

* **Interior Gateway Protocol (IGP):**
  * Used for routing **within an Autonomous System (AS)** (e.g., inside TCS or Wipro network).
  1. **Distance Vector:** RIP (Routing Information Protocol), IGRP.
  2. **Link-State:** OSPF (Open Shortest Path First), IS-IS.
  3. **Hybrid:** EIGRP (Enhanced Interior Gateway Routing Protocol).

* **Exterior Gateway Protocol (EGP):**
  * Used for routing **between different Autonomous Systems (AS)** (e.g., connecting TCS to Wipro or connecting an Enterprise to an ISP).
  * Main Protocols: **BGP (Border Gateway Protocol)**, EIGRP (for multi-AS implementations).

---

## Module 3: Router CLI Configuration & Verification

### 1. Router 1 Configuration (Mumbai Router)

Click **Router_Mumbai** $\rightarrow$ **CLI Tab**:

```text
System configuration dialog : Yes/No : no

Router> enable
Router# configure terminal
Router(config)# hostname Mumbai

! --- 1. LAN Interface Configuration (Gateway for Mumbai) ---
Mumbai(config)# interface GigabitEthernet0/0/0
Mumbai(config-if)# ip address 192.168.0.1 255.255.255.192
Mumbai(config-if)# no shutdown
Mumbai(config-if)# exit

! --- 2. WAN Interface Configuration (DCE Side) ---
Mumbai(config)# interface Serial0/1/0
Mumbai(config-if)# ip address 192.168.0.97 255.255.255.252
Mumbai(config-if)# clock rate 64000
Mumbai(config-if)# no shutdown
Mumbai(config-if)# exit

! --- 3. Static Route to Delhi LAN (192.168.0.64/27 via Next-Hop 192.168.0.98) ---
Mumbai(config)# ip route 192.168.0.64 255.255.255.224 192.168.0.98

! --- 4. Save Configuration ---
Mumbai(config)# do write memory
```

---

### 2. Router 2 Configuration (Delhi Router)

Click **Router_Delhi** $\rightarrow$ **CLI Tab**:

```text
System configuration dialog : Yes/No : no

Router> enable
Router# configure terminal
Router(config)# hostname Delhi

! --- 1. LAN Interface Configuration (Gateway for Delhi) ---
Delhi(config)# interface GigabitEthernet0/0/0
Delhi(config-if)# ip address 192.168.0.65 255.255.255.224
Delhi(config-if)# no shutdown
Delhi(config-if)# exit

! --- 2. WAN Interface Configuration (DTE Side) ---
Delhi(config)# interface Serial0/1/0
Delhi(config-if)# ip address 192.168.0.98 255.255.255.252
Delhi(config-if)# no shutdown
Delhi(config-if)# exit

! --- 3. Static Route to Mumbai LAN (192.168.0.0/26 via Next-Hop 192.168.0.97) ---
Delhi(config)# ip route 192.168.0.0 255.255.255.192 192.168.0.97

! --- 4. Save Configuration ---
Delhi(config)# do write memory
```

---

### 3. Packet Transmission & Gateway Mechanics

1. **Local Traffic Inspection:** PC0 (`192.168.0.2/26`) targets PC2 (`192.168.0.66/27`). PC0 checks its subnet mask and determines PC2 is outside its local LAN.
2. **Gateway Hand-off:** PC0 encapsulating the IP packet sends it to its Default Gateway address (`192.168.0.1` on `Router_Mumbai`).
3. **Routing Table Look-up:** `Router_Mumbai` checks its routing table (`show ip route`) for `192.168.0.64/27`. It matches the static route and forwards the frame out of `Serial0/1/0` to next-hop `192.168.0.98`.
4. **Destination Delivery:** `Router_Delhi` receives the packet on `Serial0/1/0`, identifies `192.168.0.66` as a directly connected subnet on `Gig0/0/0`, and forwards it to PC2 via `Switch_Delhi`.

---

### 4. Verification & Troubleshooting Commands

#### A. Ping Command (PC Command Prompt)
```text
C:\> ping 192.168.0.66
```
*Expected Output:*
```text
Pinging 192.168.0.66 with 32 bytes of data:
Reply from 192.168.0.66: bytes=32 time=1ms TTL=126
Reply from 192.168.0.66: bytes=32 time=1ms TTL=126
```

#### B. Router Verification Commands
* **View Routing Table:**
  ```text
  Router# show ip route
  ```
  *(Look for entries starting with `C` for directly connected networks and `S` for static routes).*

* **View Interface Brief:**
  ```text
  Router# show ip interface brief
  ```
  *(Confirms Status and Protocol are both `up`).*

---

## 5. Summary Checklist

* ✅ **Gateway Requirement:** Devices on separate subnets require a Default Gateway configured with their local router interface IP.
* ✅ **Clock Rate Setting:** Applied exclusively on the **DCE** side of a Serial connection (`clock rate 64000`).
* ✅ **Interface Activation:** Execute `no shutdown` on all configured interfaces.
* ✅ **Static Route Rule:** Routers automatically know connected networks (`C`), but require static (`S`) or dynamic routes for remote subnets.
