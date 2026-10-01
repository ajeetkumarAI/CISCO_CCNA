# Day 7: Subnetting Calculations (CIDR & Subnet Mask Formulas)

---

## 1. Core Formulas for CIDR Calculations

When calculating subnets for any IP address and CIDR prefix:

1. **Host Bits ($n$):** 
   $$n = 32 - \text{CIDR Bit}$$
2. **Subnet/Network Bits ($m$):** 
   $$m = \text{Default Host Bits of Class} - \text{Calculated Host Bits } (n)$$
3. **Number of Subnets (NOS):** 
   $$\text{NOS} = 2^m$$
4. **Number of IP Addresses Per Subnet (NOIPPS / Block Size):** 
   $$\text{NOIPPS} = 2^n$$
5. **Number of Usable Host IPs Per Subnet:** 
   $$\text{Usable Hosts} = 2^n - 2$$
6. **Block Size / Increment:** 
   $$\text{Block Size} = 256 - \text{Last Non-Zero Subnet Mask Octet Value}$$

---

## 2. Positional Bit Values (Reference Table)

$$\begin{array}{\|c\|c\|c\|c\|c\|c\|c\|c\|} \hline \text{Bit Position} & 128 & 64 & 32 & 16 & 8 & 4 & 2 & 1 \\ \hline \text{Binary Value} & 2^7 & 2^6 & 2^5 & 2^4 & 2^3 & 2^2 & 2^1 & 2^0 \\ \hline \end{array}$$

---

## 3. Class C Detailed Example (`192.168.0.0 /26`)

### Step-by-Step Breakdown
* **IP Class:** Class C (Default Host Bits = 8, Default Subnet Mask = `255.255.255.0` or `/24`)
* **Host Bits ($n$):** $32 - 26 = \mathbf{6}$
* **Network Bits ($m$):** $8 - 6 = \mathbf{2}$
* **Number of Subnets (NOS):** $2^m = 2^2 = \mathbf{4}$
* **Usable IPs per Subnet:** $2^n - 2 = 2^6 - 2 = 64 - 2 = \mathbf{62}$
* **Subnet Mask for `/26`:** 
  `11111111 . 11111111 . 11111111 . 11000000` $\rightarrow$ **`255.255.255.192`**
* **Block Size:** $256 - 192 = \mathbf{64}$

### Subnet Ranges
1. **Subnet 1:** `192.168.0.0` to `192.168.0.63` `/26`
2. **Subnet 2:** `192.168.0.64` to `192.168.0.127` `/26`
3. **Subnet 3:** `192.168.0.128` to `192.168.0.191` `/26`
4. **Subnet 4:** `192.168.0.192` to `192.168.0.255` `/26`

---

## 4. Class Question Solution (`192.168.0.149 /26`)

Given the target IP **`192.168.0.149 /26`**:

* **Class:** **Class C** (First octet 192)
* **Subnet Mask (SM):** **`255.255.255.192`**
* **Subnet Range:** Fits into Subnet 3 (`192.168.0.128` – `192.168.0.191`)
* **Network ID (NID):** **`192.168.0.128`**
* **Broadcast ID (BID):** **`192.168.0.191`**
* **Valid Host Range:** `192.168.0.129` – `192.168.0.190`
* **Is `192.168.0.149` a Valid Host IP?** **Yes (Valid)**

---

## 5. Homework Assignments Solved (`/25`, `/27`, `/28`, `/29`, `/30`)

### 1. Homework: `/25` Mask (`192.168.0.0 /25`)
* **Host Bits ($n$):** $32 - 25 = 7$
* **Network Bits ($m$):** $8 - 7 = 1$
* **NOS:** $2^1 = \mathbf{2}$
* **Block Size:** $2^7 = \mathbf{128}$ ($256 - 128 = 128$)
* **Subnet Mask:** **`255.255.255.128`**
* **Usable Hosts:** $128 - 2 = \mathbf{126}$
* **Ranges:**
  * Subnet 1: `192.168.0.0` – `192.168.0.127`
  * Subnet 2: `192.168.0.128` – `192.168.0.255`

---

### 2. Homework: `/27` Mask (`192.168.0.0 /27`)
* **Host Bits ($n$):** $32 - 27 = 5$
* **Network Bits ($m$):** $8 - 5 = 3$
* **NOS:** $2^3 = \mathbf{8}$
* **Block Size:** $2^5 = \mathbf{32}$ ($256 - 224 = 32$)
* **Subnet Mask:** **`255.255.255.224`**
* **Usable Hosts:** $32 - 2 = \mathbf{30}$
* **Ranges:** `0-31`, `32-63`, `64-95`, `96-127`, `128-159`, `160-191`, `192-223`, `224-255`

---

### 3. Homework: `/28` Mask (`192.168.0.0 /28`)
* **Host Bits ($n$):** $32 - 28 = 4$
* **Network Bits ($m$):** $8 - 4 = 4$
* **NOS:** $2^4 = \mathbf{16}$
* **Block Size:** $2^4 = \mathbf{16}$ ($256 - 240 = 16$)
* **Subnet Mask:** **`255.255.255.240`**
* **Usable Hosts:** $16 - 2 = \mathbf{14}$
* **Ranges:** `0-15`, `16-31`, `32-47`, ..., `240-255` (Increments of 16)

---

### 4. Homework: `/29` Mask (`192.168.0.0 /29`)
* **Host Bits ($n$):** $32 - 29 = 3$
* **Network Bits ($m$):** $8 - 3 = 5$
* **NOS:** $2^5 = \mathbf{32}$
* **Block Size:** $2^3 = \mathbf{8}$ ($256 - 248 = 8$)
* **Subnet Mask:** **`255.255.255.248`**
* **Usable Hosts:** $8 - 2 = \mathbf{6}$
* **Ranges:** `0-7`, `8-15`, `16-23`, ..., `248-255` (Increments of 8)

---

### 5. Homework: `/30` Mask (`192.168.0.0 /30`)
> **Note:** `/30` is standard for Point-to-Point serial link router connections.

* **Host Bits ($n$):** $32 - 30 = 2$
* **Network Bits ($m$):** $8 - 2 = 6$
* **NOS:** $2^6 = \mathbf{64}$
* **Block Size:** $2^2 = \mathbf{4}$ ($256 - 252 = 4$)
* **Subnet Mask:** **`255.255.255.252`**
* **Usable Hosts:** $4 - 2 = \mathbf{2}$
* **Ranges:** `0-3`, `4-7`, `8-11`, ..., `252-255` (Increments of 4)

---

## 6. Subnetting for Class B & Class A

### Class B Example (`172.16.0.0 /18`)
* **Default Host Bits:** 16 (Class B default `/16`)
* **Host Bits ($n$):** $32 - 18 = 14$
* **Network Bits ($m$):** $16 - 14 = 2$
* **NOS:** $2^2 = \mathbf{4}$
* **Subnet Mask:** **`255.255.192.0`**
* **Block Size:** Changes in 3rd Octet by $256 - 192 = \mathbf{64}$
* **Ranges:**
  1. `172.16.0.0` – `172.16.63.255`
  2. `172.16.64.0` – `172.16.127.255`
  3. `172.16.128.0` – `172.16.191.255`
  4. `172.16.192.0` – `172.16.255.255`

---

### Class A Example (`10.0.0.0 /10`)
* **Default Host Bits:** 24 (Class A default `/8`)
* **Host Bits ($n$):** $32 - 10 = 22$
* **Network Bits ($m$):** $24 - 22 = 2$
* **NOS:** $2^2 = \mathbf{4}$
* **Subnet Mask:** **`255.192.0.0`**
* **Block Size:** Changes in 2nd Octet by $256 - 192 = \mathbf{64}$
* **Ranges:**
  1. `10.0.0.0` – `10.63.255.255`
  2. `10.64.0.0` – `10.127.255.255`
  3. `10.128.0.0` – `10.191.255.255`
  4. `10.192.0.0` – `10.255.255.255`
