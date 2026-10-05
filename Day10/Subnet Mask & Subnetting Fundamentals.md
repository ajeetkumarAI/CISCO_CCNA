# Day 10: Subnet Mask & Subnetting Fundamentals

---

## 1. Subnet Mask
**Definition:** Used to identify the **Network Portion** of an IP Address.

### Default Subnet Masks
* **Class A:** `255.0.0.0 /8` $\rightarrow$ Structure: `N . H . H . H`
* **Class B:** `255.255.0.0 /16` $\rightarrow$ Structure: `N . N . H . H`
* **Class C:** `255.255.255.0 /24` $\rightarrow$ Structure: `N . N . N . H`

> **Note:**
> * $N \implies \text{Network Bit } (1)$
> * $H \implies \text{Host Bit } (0)$

---

## 2. Binary Representation of Subnet Masks

### Class A (`/8`)
```text
11111111 . 00000000 . 00000000 . 00000000 /8
  255    .    0     .    0     .    0

```

### Class B (`/16`)

```text
11111111 . 11111111 . 00000000 . 00000000 /16
  255    .   255    .    0     .    0

```

### Class C (`/24`)

```text
11111111 . 11111111 . 11111111 . 00000000 /24
  255    .   255    .   255    .    0

```

---

## 3. Allowed vs. Not Allowed Values

| Allow Value (Valid) | Not Allow Value (Invalid) |
| --- | --- |
| **Network = /CIDR** | **Network = /CIDR** |
| **Class A** = `/B, C` (e.g., `/16`, `/24`) | **Class C** = `/B, A` (e.g., `/16`, `/8`) |
| **Class B** = `/C` (e.g., `/24`) | **Class B** = `/A` (e.g., `/8`) |

---

## 4. Subnetting Overview

**Definition:** A process to divide a large network into multiple smaller networks.

### Class of Subnetting:

1. **CIDR (Classless Inter-Domain Routing):** As per **given bit** provided by a large network to divide into small networks with their limitation.

$$\text{Given bit} = \text{CIDR bit} = \text{Network bit}$$


2. **VLSM (Variable Length Subnet Mask):** As per **requirement**.

---

## 5. CIDR Range by Class

* **Class A Range:** `/8` to `/15`
* **Class B Range:** `/16` to `/23`
* **Class C Range:** `/24` to `/30`

---

## 6. Types of Subnetting

### 1. Classfull Subnetting

**Definition:** IP Address and Subnet Mask are both in the **same class**.

* `10.0.0.0 /8` $\implies$ `255.0.0.0` (Class A IP + Class A Mask)
* `172.16.0.0 /16` $\implies$ `255.255.0.0` (Class B IP + Class B Mask)
* `192.168.0.0 /24` $\implies$ `255.255.255.0` (Class C IP + Class C Mask)

---

### 2. Classless Subnetting

**Definition:** IP Address and Subnet Mask are in **different classes**.

* `10.0.0.0 /16` $\implies$ `255.255.0.0` (Class A IP + Class B Mask)
* `10.0.0.0 /24` $\implies$ `255.255.255.0` (Class A IP + Class C Mask)
* `172.16.0.0 /25` $\implies$ `255.255.255.128` (Class B IP + Class C Mask)

---

## 7. Network Subnet Allocation Diagram (Department Host Requirements)

```text
+-----------------------+     +-----------------------+
|                       |     |                       |
|       50 Hosts        |     |       20 Hosts        |
|                       |     |                       |
+-----------------------+     +-----------------------+

+-----------------------+     +-----------------------+
|                       |     |                       |
|      100 Hosts        |     |       40 Hosts        |
|                       |     |                       |
+-----------------------+     +-----------------------+

```

```

```
