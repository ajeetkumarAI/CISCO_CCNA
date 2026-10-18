
# OSPF Multi-Area, ABR, ASBR & Autonomous Systems

---

## 1. Network Design & VLSM IP Allocation Plan

We are setting up a multi-site enterprise network with two branch offices (**Mumbai** and **Delhi**) connected via a **WAN serial link**.

### Host & Subnet Requirements:
* **Mumbai LAN:** Requires 40 Host IPs $\rightarrow$ Needs a `/26` subnet (62 usable hosts).
* **Delhi LAN:** Requires 20 Host IPs $\rightarrow$ Needs a `/27` subnet (30 usable hosts).
* **WAN Link (Mumbai to Delhi):** Requires 2 IP addresses $\rightarrow$ Needs a `/30` subnet (2 usable hosts).

### IP Address Allocation Table:
| Network Segment | Subnet / CIDR | Subnet Mask | Usable Host Range | Gateway / Router IP |
| :--- | :--- | :--- | :--- | :--- |
| **Mumbai LAN** | `192.168.0.0 /26` | `255.255.255.192` | `192.168.0.1` – `192.168.0.62` | `192.168.0.1` |
| **Delhi LAN** | `192.168.0.64 /27` | `255.255.255.224` | `192.168.0.65` – `192.168.0.94` | `192.168.0.65` |
| **WAN Serial Link** | `192.168.0.96 /30` | `255.255.255.252` | `192.168.0.97` – `192.168.0.98` | Router 0: `.97`<br>Router 1: `.98` |

---

## 2. Step-by-Step Packet Tracer Setup Instructions

### Step 1: Place Devices on the Workspace
1. **Routers:** Place 2 Routers (e.g., `Router0` for Mumbai, `Router1` for Delhi). *(Note: Ensure routers have serial interface modules installed, such as HWIC-2T, if using serial connections).*
2. **Switches:** Place 2 Switches (`Switch0` and `Switch1`).
3. **End Devices:** Place 4 PCs (`PC0`, `PC1` connected to Mumbai switch; `PC2`, `PC3` connected to Delhi switch).

### Step 2: Physical Cabling
* **PC to Switch:** Use **Copper Straight-Through** cables (Connect PC NIC to Switch FastEthernet ports like `Fa0/1`, `Fa0/2`).
* **Switch to Router:** Use **Copper Straight-Through** cables (Connect Switch uplink to Router GigabitEthernet port, e.g., `Gi0/0/0`).
* **Router to Router (WAN):** Use **Serial DCE** cable (Connect Router0 `Se0/1/0` to Router1 `Se0/0`). *Remember: The end with the clock icon is the DCE side.*

---

## 3. Configuring IP Addresses on End Devices (PCs)

Click on each PC $\rightarrow$ **Desktop Tab** $\rightarrow$ **IP Configuration**:

### Mumbai PCs (Gateway: `192.168.0.1`)
* **PC0:** 
  * IP Address: `192.168.0.2`
  * Subnet Mask: `255.255.255.192`
  * Default Gateway: `192.168.0.1`
* **PC1:** 
  * IP Address: `192.168.0.3`
  * Subnet Mask: `255.255.255.192`
  * Default Gateway: `192.168.0.1`

### Delhi PCs (Gateway: `192.168.0.65`)
* **PC2:** 
  * IP Address: `192.168.0.66`
  * Subnet Mask: `255.255.255.224`
  * Default Gateway: `192.168.0.65`
* **PC3:** 
  * IP Address: `192.168.0.67`
  * Subnet Mask: `255.255.255.224`
  * Default Gateway: `192.168.0.65`

---

## 4. Router CLI Configuration Commands

Click on each router $\rightarrow$ **CLI Tab** and run the following commands:

### Router 0 (Mumbai Router) Configuration:
```text
Router> enable
Router# configure terminal
Router(config)# hostname Mumbai-Rtr

! --- Configure LAN Interface (GigabitEthernet 0/0/0) ---
Mumbai-Rtr(config)# interface gigabitethernet 0/0/0
Mumbai-Rtr(config-if)# ip address 192.168.0.1 255.255.255.192
Mumbai-Rtr(config-if)# no shutdown
Mumbai-Rtr(config-if)# exit

! --- Configure WAN Serial Interface (Serial 0/1/0) ---
Mumbai-Rtr(config)# interface serial 0/1/0
Mumbai-Rtr(config-if)# ip address 192.168.0.97 255.255.255.252
Mumbai-Rtr(config-if)# clock rate 64000
Mumbai-Rtr(config-if)# no shutdown
Mumbai-Rtr(config-if)# exit

```

### Router 1 (Delhi Router) Configuration:

```text
Router> enable
Router# configure terminal
Router(config)# hostname Delhi-Rtr

! --- Configure LAN Interface (GigabitEthernet 0/0/0) ---
Delhi-Rtr(config)# interface gigabitethernet 0/0/0
Delhi-Rtr(config-if)# ip address 192.168.0.65 255.255.255.224
Delhi-Rtr(config-if)# no shutdown
Delhi-Rtr(config-if)# exit

! --- Configure WAN Serial Interface (Serial 0/0) ---
Delhi-Rtr(config)# interface serial 0/0
Delhi-Rtr(config-if)# ip address 192.168.0.98 255.255.255.252
Delhi-Rtr(config-if)# no shutdown
Delhi-Rtr(config-if)# exit

```

---

## 5. Setting Up Inter-Network Communication (Routing)

Since Mumbai and Delhi are on separate networks, the routers need to know how to reach each other. You can use **Static Routing** or **OSPF Dynamic Routing**.

### Option A: Static Routing Configuration

* **On Mumbai Router (`Mumbai-Rtr`):**
```text
Mumbai-Rtr(config)# ip route 192.168.0.64 255.255.255.224 192.168.0.98

```


* **On Delhi Router (`Delhi-Rtr`):**
```text
Delhi-Rtr(config)# ip route 192.168.0.0 255.255.255.192 192.168.0.97

```



### Option B: OSPF Dynamic Routing Configuration (Recommended)

* **On Mumbai-Rtr:**
```text
Mumbai-Rtr(config)# router ospf 1
Mumbai-Rtr(config-router)# network 192.168.0.0 0.0.0.63 area 0
Mumbai-Rtr(config-router)# network 192.168.0.96 0.0.0.3 area 0
Mumbai-Rtr(config-router)# end

```


* **On Delhi-Rtr:**
```text
Delhi-Rtr(config)# router ospf 1
Delhi-Rtr(config-router)# network 192.168.0.64 0.0.0.31 area 0
Delhi-Rtr(config-router)# network 192.168.0.96 0.0.0.3 area 0
Delhi-Rtr(config-router)# end

```



Save configurations on both routers:

```text
Router# write memory

```

---

## 6. Testing & Verification

1. **Check Router Interface Status:**
```text
Router# show ip interface brief

```


*(Ensure all LAN and WAN interfaces show status `up` and protocol `up`).*
2. **Check Routing Table:**
```text
Router# show ip route

```


3. **Test Connectivity (Ping Test):**
* Open **PC0** (Mumbai) $\rightarrow$ **Desktop** $\rightarrow$ **Command Prompt**.
* Ping PC2 in Delhi:
```text
ping 192.168.0.66

```


* If configured correctly, replies will be successful!



```

```
