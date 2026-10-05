# Router Modes, Cabling & WAN Configuration Guide

---

## 1. Cabling Standards (Quick Cheat Sheet)

| Cable Type | When to Use | Examples |
| :--- | :--- | :--- |
| **Straight-Through Cable** | Connects **dissimilar** devices | PC to Switch, Router to Switch |
| **Cross-Over Cable** | Connects **similar** devices | Router to Router, Switch to Switch, PC to PC |
| **Rollover Cable (Console Cable)** | Initial device configuration | PC (COM port) to Router/Switch Console Port |

---

## 2. Router CLI Modes & Key Commands

A Cisco router uses a hierarchy of command modes:

```text
 [Setup Mode] ──(Type 'no')──> [User Mode] (Router>) ──('enable')──> [Privilege Mode] (Router#)
                                                                            │
                                                                 ('configure terminal')
                                                                            │
                                                                            ▼
 [Interface Mode] (Router(config-if)#) <──('interface ...')── [Global Config Mode] (Router(config)#)
```

### The 5 Command Modes
1. **Setup Mode:** Initial prompt (`System configuration dialog : Yes/No : No`). Type `no` to bypass.
2. **User Mode (`Router>`):** Basic monitoring/viewing.
   * *Command to enter Privilege Mode:* `enable` (or `en`)
3. **Privileged Mode (`Router#`):** Viewing detailed system info and saving configs.
   * *Command to enter Global Config Mode:* `configure terminal` (or `conf t`)
4. **Global Configuration Mode (`Router(config)#`):** Global router settings.
   * *Command to enter Interface Mode:* `interface Serial 0/1/0` (or `int S0/1/0`)
5. **Interface Configuration Mode (`Router(config-if)#`):** Port-level settings (IP address, clock rate).

---

### Essential Configuration Commands

```text
! Step 1: Assign IP Address and Subnet Mask
Router(config-if)# ip address 192.168.0.97 255.255.255.252

! Step 2: Set Clock Rate (REQUIRED ONLY ON THE DCE END)
Router(config-if)# clock rate 64000

! Step 3: Turn on the Interface
Router(config-if)# no shutdown   (Short: no sh)

! Step 4: Go back to Previous Mode
Router(config-if)# exit

! Step 5: Save Configuration to NVRAM
Router# write memory             (Short: wr)

! Step 6: Verification Commands
Router# show running-config      (Short: sh run)
Router# show ip interface brief  (Short: sh ip int br)
```

---

## 3. Interface Naming Conventions & DCE vs. DTE

### Interface Naming Structure: `Module / Slot / Port` (e.g., `S0/1/0`)
* **Serial Interfaces (WAN):** Denoted as `Serial S/P` or `M/S/P` (e.g., `0/1/0`).
* **Ethernet Interfaces (LAN / WAN):** 
  * `Ethernet` (10 Mbps)
  * `FastEthernet` (100 Mbps)
  * `GigabitEthernet` (1 Gbps $\rightarrow$ e.g., `G0/0/0`)

### DCE vs. DTE Serial Links
* **DCE (Data Communications Equipment):** Provides clocking synchronization for the link. **MUST have `clock rate 64000` configured.**
* **DTE (Data Terminal Equipment):** Receives clocking from the DCE end.
* *Rule:* On a Serial back-to-back connection, one side is **DCE** and the other side is **DTE**.

```text
 [ Router Pune ]  =========================================  [ Router Delhi ]
   (DCE End)                Serial WAN Cable                   (DTE End)
 (clock rate 64000)                                         (Receives clock)
```

---

## 4. WAN Topology Subnetting Breakdown

### Network Topology Scenario
* **LAN 2 (Delhi Branch):** Requires **40 Hosts**
* **LAN 1 (Pune Branch):** Requires **25 Hosts**
* **WAN Link (Pune ↔ Delhi):** Requires **2 Host IPs** (Point-to-Point)

```text
         [ Router Pune ] <=========================> [ Router Delhi ]
          (G0/0/0) (S0/1/0)     WAN=2 (192.168.0.96/30)   (S0/1/0)
             │                                               │
          [ Switch ]                                      [ Switch ]
             │                                               │
      LAN-1 (25 Hosts)                                LAN-2 (40 Hosts)
    192.168.0.64/27                                 192.168.0.0/26
```

---

### Step-by-Step VLSM Allocation (Largest to Smallest Requirement)

#### 1. LAN 2 (Delhi Branch) $\rightarrow$ 40 Hosts
* **Target Size:** Needs at least 40 hosts $\rightarrow$ Block size = **64** ($2^6 = 64$).
* **CIDR Prefix:** `/26` ($32 - 6 = 26$)
* **Subnet Mask:** `255.255.255.192`
* **Network Range:** **`192.168.0.0` to `192.168.0.63 /26`**
  * **NID:** `192.168.0.0`
  * **BID:** `192.168.0.63`
  * **Valid Usable Hosts:** `192.168.0.1` – `192.168.0.62`

#### 2. LAN 1 (Pune Branch) $\rightarrow$ 25 Hosts
* **Target Size:** Needs at least 25 hosts $\rightarrow$ Block size = **32** ($2^5 = 32$).
* **CIDR Prefix:** `/27` ($32 - 5 = 27$)
* **Subnet Mask:** `255.255.255.224`
* **Network Range:** **`192.168.0.64` to `192.168.0.95 /27`**
  * **NID:** `192.168.0.64`
  * **BID:** `192.168.0.95`
  * **Valid Usable Hosts:** `192.168.0.65` – `192.168.0.94`

#### 3. WAN Link (Pune to Delhi Serial) $\rightarrow$ 2 Hosts
* **Target Size:** Point-to-Point Serial link $\rightarrow$ Block size = **4** ($2^2 = 4$).
* **CIDR Prefix:** `/30` ($32 - 2 = 30$)
* **Subnet Mask:** `255.255.255.252`
* **Network Range:** **`192.168.0.96` to `192.168.0.99 /30`**
  * **NID:** `192.168.0.96`
  * **BID:** `192.168.0.99`
  * **Usable Host IPs:** `192.168.0.97` (Pune `S0/1/0`) and `192.168.0.98` (Delhi `S0/1/0`)

---

## 5. Pro Tricks & Common Pitfalls to Avoid ⚠️

### Pro Tricks 💡
1. **Always Allocate Largest Subnets First:** When doing Variable Length Subnet Masking (VLSM), start with the largest host requirement (`40` $\rightarrow$ `25` $\rightarrow$ `2`) to prevent subnet overlap.
2. **Use Command Shortcuts:** Use `conf t`, `int s0/1/0`, `no sh`, `sh ip int br`, and `wr` in Cisco CLI to configure twice as fast.
3. **Point-to-Point Rule:** Always use `/30` for point-to-point serial links because it provides exactly **2 usable IP addresses** with zero wasted space.

### Pitfalls to Avoid ⚠️
1. ❌ **Forgetting `no shutdown`:** Cisco interfaces are turned off by default (`administratively down`). Always run `no shutdown` after configuring IP addresses.
2. ❌ **Missing `clock rate` on DCE:** Serial links will stay down (`protocol down`) if the DCE end is missing the `clock rate 64000` command.
3. ❌ **Forgetting `write memory` (`wr`):** If you reboot the router without running `wr`, all running configurations will be lost!
