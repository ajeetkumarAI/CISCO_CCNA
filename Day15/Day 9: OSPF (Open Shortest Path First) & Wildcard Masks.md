# Day 9: OSPF (Open Shortest Path First) & Wildcard Masks

---

## 1. What is OSPF?
**OSPF (Open Shortest Path First)** is a dynamic routing protocol used to automatically discover and exchange routing information between routers in a large network.

### Key Characteristics of OSPF:
* **Type:** Link-State Routing Protocol.
* **Scope:** Supports large enterprise networks.
* **No Hop Count Limit:** Unlike RIP (which maxes out at 15 hops), OSPF has no hop limit.
* **Mask Support:** Fully supports Classful, Classless, VLSM, and **Wildcard Masks**.
* **Updates:** Sends **incremental and triggered updates** (only sends updates when a network change occurs, saving bandwidth).
* **Multicast Addresses Used:** 
  * `224.0.0.5` (All OSPF Routers)
  * `224.0.0.6` (Designated Routers / Backup Designated Routers)
* **Algorithm:** Uses **Dijkstra's Algorithm** (Shortest Path First) to calculate the best route.
* **Metric:** **Cost** (based on interface bandwidth).
* **Features:** Supports Equal-Cost Load Balancing and MD5/Plaintext authentication.

---

## 2. Wildcard Mask (Inverse Subnet Mask)

A **Wildcard Mask** is the exact opposite (reverse form) of a Subnet Mask. It tells OSPF which IP bits to check (`0`) and which bits to ignore (`255`).

* **Subnet Mask rule:** `255` = Network bit, `0` = Host bit
* **Wildcard Mask rule:** `0` = Network bit (must match), `255` = Host bit (don't care)

### Formula:
$$\text{Wildcard Mask} = \text{Broadcast Mask } (255.255.255.255) - \text{Subnet Mask}$$

### Examples:
* **Class A Subnet Mask:** `255.0.0.0` $\rightarrow$ **Wildcard Mask:** `0.255.255.255`
* **Class B Subnet Mask:** `255.255.0.0` $\rightarrow$ **Wildcard Mask:** `0.0.255.255`
* **Class C Subnet Mask:** `255.255.255.0` $\rightarrow$ **Wildcard Mask:** `0.0.0.255`
* **`/30` Subnet Mask (`255.255.255.252`):** $\rightarrow$ **Wildcard Mask:** `0.0.0.3`

---

## 3. OSPF Metric & Cost Calculation

OSPF uses **Cost** as its metric to determine the best path. Cost is inversely proportional to bandwidth (higher bandwidth = lower cost = preferred path).

### Formula:
$$\text{Cost} = \frac{\text{Reference Bandwidth}}{\text{Interface Bandwidth (bps)}}$$

*(Default Cisco Reference Bandwidth is $10^8$ bps / 100 Mbps)*

### Example Calculation (Serial Link with $1.544\text{ Mbps}$ / T1 line):
$$\text{Cost} = \frac{10^8}{1.544 \times 10^6} = \frac{100,000,000}{1,544,000} \approx \mathbf{64}$$

---

## 4. OSPF Routing Table Indicators
When you run `show ip route` on an OSPF router:
* **`C`** $\implies$ Directly Connected Network
* **`S`** $\implies$ Static Route
* **`O`** $\implies$ OSPF Dynamic Route Learned from Neighbor

---

## 5. Step-by-Step OSPF Configuration in Packet Tracer CLI

To configure OSPF between two routers connected via serial and LAN interfaces:

### Router 0 Configuration:
```text
Router> enable
Router# configure terminal
Router(config)# router ospf 1
Router(config-router)# network 10.0.0.0 0.0.0.255 area 0
Router(config-router)# network 20.0.0.0 0.0.0.3 area 0
Router(config-router)# end
Router# write memory

```

### Router 1 Configuration:

```text
Router> enable
Router# configure terminal
Router(config)# router ospf 1
Router(config-router)# network 30.0.0.0 0.0.0.255 area 0
Router(config-router)# network 20.0.0.0 0.0.0.3 area 0
Router(config-router)# end
Router# write memory

```

---

## 6. Verification Commands

To verify OSPF neighbors and learned routes:

* **Check OSPF Routing Table:**
```text
Router# show ip route

```


* **Check OSPF Neighbor Adjacency:**
```text
Router# show ip ospf neighbor

```
