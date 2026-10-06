# Cisco Packet Tracer Lab Guide: 3-Subnet Network Setup (Mumbai, Delhi & WAN)

---

## 1. Lab Scenario & Requirements

You need to connect two branch offices—**Mumbai** and **Delhi**—over a WAN link using **Cisco Packet Tracer**.

### Requirements:
1. **Mumbai LAN (LAN 1):** Needs **40 IP addresses** for host devices.
2. **Delhi LAN (LAN 2):** Needs **20 IP addresses** for host devices.
3. **WAN Link (Serial Link):** Connects Mumbai Router to Delhi Router (**2 IP addresses** needed).
4. **Base IP Address Block:** `192.168.0.0 /24`

---

## 2. VLSM IP Address Calculation Table

Using **Variable Length Subnet Masking (VLSM)**, always allocate IP blocks starting with the largest host requirement first:

| Network Name | Required IPs | Block Size ($2^n$) | CIDR Prefix | Subnet Mask | Network ID (NID) | Usable Host Range | Default Gateway (Router IP) | Broadcast ID (BID) |
| :--- | :---: | :---: | :---: | :--- | :--- | :--- | :--- | :--- |
| **1. Mumbai LAN** | 40 | 64 ($2^6$) | `/26` | `255.255.255.192` | `192.168.0.0` | `192.168.0.1` – `192.168.0.62` | `192.168.0.1` | `192.168.0.63` |
| **2. Delhi LAN** | 20 | 32 ($2^5$) | `/27` | `255.255.255.224` | `192.168.0.64` | `192.168.0.65` – `192.168.0.94` | `192.168.0.65` | `192.168.0.95` |
| **3. WAN Link** | 2 | 4 ($2^2$) | `/30` | `255.255.255.252` | `192.168.0.96` | `192.168.0.97` – `192.168.0.98` | N/A (Point-to-Point) | `192.168.0.99` |

---

## 3. Physical Devices & Cabling Setup

### Devices to Drag and Drop in Packet Tracer:
* **2x Routers:** 1941 or 2911 model (Name them **Router_Mumbai** and **Router_Delhi**).
* **2x Switches:** 2960 model (Name them **Switch_Mumbai** and **Switch_Delhi**).
* **4x End Devices (PCs):** PC0, PC1 in Mumbai; PC2, PC3 in Delhi.

> ⚠️ **Important (Adding Serial Interfaces):**
> Cisco 1941 / 2911 routers do not have Serial ports by default. 
> 1. Click on the Router $\rightarrow$ **Physical Tab**.
> 2. Turn off the **Power Switch**.
> 3. Drag the **HWIC-2T** module into the empty slot.
> 4. Turn the **Power Switch** back ON.

---

### Cabling Connection Guide:

| Source Device & Port | Destination Device & Port | Cable Type | Reason / Rule |
| :--- | :--- | :--- | :--- |
| **PC0 (FastEthernet0)** | **Switch_Mumbai (Fa0/2)** | Straight-Through Cable | Dissimilar Devices |
| **PC1 (FastEthernet0)** | **Switch_Mumbai (Fa0/3)** | Straight-Through Cable | Dissimilar Devices |
| **Switch_Mumbai (Fa0/1)**| **Router_Mumbai (Gig0/0/0)** | Straight-Through Cable | Dissimilar Devices |
| **PC2 (FastEthernet0)** | **Switch_Delhi (Fa0/2)** | Straight-Through Cable | Dissimilar Devices |
| **PC3 (FastEthernet0)** | **Switch_Delhi (Fa0/3)** | Straight-Through Cable | Dissimilar Devices |
| **Switch_Delhi (Fa0/1)** | **Router_Delhi (Gig0/0/0)** | Straight-Through Cable | Dissimilar Devices |
| **Router_Mumbai (Se0/1/0)**| **Router_Delhi (Se0/1/0)** | Serial DCE Cable | Router-to-Router WAN Link |

---

## 4. End-Device (PC) IP & Gateway Configuration

Click on each PC $\rightarrow$ **Desktop Tab** $\rightarrow$ **IP Configuration**:

### A. Mumbai PCs (Subnet: `192.168.0.0/26`)
* **PC0 Configuration:**
  * **IP Address:** `192.168.0.2`
  * **Subnet Mask:** `255.255.255.192`
  * **Default Gateway:** `192.168.0.1`

* **PC1 Configuration:**
  * **IP Address:** `192.168.0.3`
  * **Subnet Mask:** `255.255.255.192`
  * **Default Gateway:** `192.168.0.1`

### B. Delhi PCs (Subnet: `192.168.0.64/27`)
* **PC2 Configuration:**
  * **IP Address:** `192.168.0.66`
  * **Subnet Mask:** `255.255.255.224`
  * **Default Gateway:** `192.168.0.65`

* **PC3 Configuration:**
  * **IP Address:** `192.168.0.67`
  * **Subnet Mask:** `255.255.255.224`
  * **Default Gateway:** `192.168.0.65`

---

## 5. Step-by-Step Router CLI Commands

### Router 1: Mumbai Router Configuration

Click on **Router_Mumbai** $\rightarrow$ **CLI Tab**:

```text
System configuration dialog : Yes/No : no

Router> enable
Router# configure terminal
Router(config)# hostname Mumbai

! 1. Configure LAN Interface (Gig0/0/0 - Default Gateway for Mumbai)
Mumbai(config)# interface GigabitEthernet0/0/0
Mumbai(config-if)# ip address 192.168.0.1 255.255.255.192
Mumbai(config-if)# no shutdown
Mumbai(config-if)# exit

! 2. Configure WAN Interface (Se0/1/0 - Serial DCE End)
Mumbai(config)# interface Serial0/1/0
Mumbai(config-if)# ip address 192.168.0.97 255.255.255.252
Mumbai(config-if)# clock rate 64000
Mumbai(config-if)# no shutdown
Mumbai(config-if)# exit

! 3. Configure Static Route to Reach Delhi LAN (192.168.0.64/27)
Mumbai(config)# ip route 192.168.0.64 255.255.255.224 192.168.0.98

! 4. Save Configuration
Mumbai(config)# do write memory
```

---

### Router 2: Delhi Router Configuration

Click on **Router_Delhi** $\rightarrow$ **CLI Tab**:

```text
System configuration dialog : Yes/No : no

Router> enable
Router# configure terminal
Router(config)# hostname Delhi

! 1. Configure LAN Interface (Gig0/0/0 - Default Gateway for Delhi)
Delhi(config)# interface GigabitEthernet0/0/0
Delhi(config-if)# ip address 192.168.0.65 255.255.255.224
Delhi(config-if)# no shutdown
Delhi(config-if)# exit

! 2. Configure WAN Interface (Se0/1/0 - Serial DTE End)
Delhi(config)# interface Serial0/1/0
Delhi(config-if)# ip address 192.168.0.98 255.255.255.252
Delhi(config-if)# no shutdown
Delhi(config-if)# exit

! 3. Configure Static Route to Reach Mumbai LAN (192.168.0.0/26)
Delhi(config)# ip route 192.168.0.0 255.255.255.192 192.168.0.97

! 4. Save Configuration
Delhi(config)# do write memory
```

---

## 6. How Communication Works Across Networks (Verification)

### Understanding the Role of Default Gateway:
1. When **PC0 (`192.168.0.2`)** wants to ping **PC2 (`192.168.0.66`)**, it checks if the destination IP is in its own subnet (`192.168.0.0/26`).
2. Since **PC2** is in a different network (`192.168.0.64/27`), **PC0 sends the packet to its Default Gateway (`192.168.0.1`)**.
3. **Router_Mumbai** receives the packet, checks its Routing Table (`ip route`), and forwards it across the WAN link (`192.168.0.97` $\rightarrow$ `192.168.0.98`) to **Router_Delhi**.
4. **Router_Delhi** delivers the packet locally to **PC2**.

---

## 7. Lab Verification Commands & Testing

### A. Testing Ping from PC0 (Mumbai) to PC2 (Delhi)
1. Click **PC0** $\rightarrow$ **Desktop** $\rightarrow$ **Command Prompt**.
2. Run the command:
   ```cmd
   ping 192.168.0.66
   ```
3. *Expected Result:* First packet might time out (ARP request), followed by **Reply from 192.168.0.66: bytes=32 time<1ms TTL=126**.

---

### B. Helpful CLI Troubleshooting Commands

* **Check interface statuses & assigned IPs:**
  ```text
  Router# show ip interface brief
  ```
  *(Verify status is **up/up** for Gig0/0/0 and Se0/1/0).*

* **Verify Routing Table:**
  ```text
  Router# show ip route
  ```
  *(Verify static route `S 192.168.0.xx` is present).*

---

## 8. Summary Checklist of Golden Rules 🌟

* ✅ **Gateway Required:** Devices in different subnets **cannot** talk to each other without a Default Gateway set to the local router IP interface.
* ✅ **Clock Rate Rule:** Only the **DCE** side of the serial cable needs `clock rate 64000`.
* ✅ **No Shutdown:** Always type `no shutdown` on router ports; otherwise, links remain red.
* ✅ **Static Routing (`ip route`):** Routers need to be told how to reach remote subnets via `ip route [Destination Network ID] [Subnet Mask] [Next-Hop IP]`.
