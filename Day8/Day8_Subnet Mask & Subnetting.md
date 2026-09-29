```markdown
# Subnet Mask & Subnetting (Simplified Guide)

---

## 1. Real-World Analogy: What is a Subnet Mask?

Imagine an **IP address** is like a mailing address:
* **Network Portion:** The **City Name** (helps mail travel between locations).
* **Host Portion:** The **House Number** (identifies the exact building).

A **Subnet Mask** is simply a highlighter tool that tells a computer:
> *"This highlighted part is the City Name (Network), and the rest is the House Number (Host)."*

---

## 2. Default Subnet Masks & Binary View

In binary logic:
* **Bit `1`** = **Network Bit** (Highlighted / Fixed)
* **Bit `0`** = **Host Bit** (Variable / Free for devices)

---

### Class A
* **Default Subnet Mask:** `255.0.0.0`
* **CIDR Notation:** `/8` (8 ON bits)
* **Binary Representation:**
  ```text
  11111111 . 00000000 . 00000000 . 00000000
    255    .    0     .    0     .    0

```

* **Usable Host Capacity:** `1,67,77,214` devices

---

### Class B

* **Default Subnet Mask:** `255.255.0.0`
* **CIDR Notation:** `/16` (16 ON bits)
* **Binary Representation:**
```text
11111111 . 11111111 . 00000000 . 00000000
  255    .   255    .    0     .    0

```


* **Usable Host Capacity:** `65,534` devices

---

### Class C

* **Default Subnet Mask:** `255.255.255.0`
* **CIDR Notation:** `/24` (24 ON bits)
* **Binary Representation:**
```text
11111111 . 11111111 . 11111111 . 00000000
  255    .   255    .   255    .    0

```


* **Usable Host Capacity:** `254` devices

---

## 3. What is Subnetting?

**Subnetting** is the process of taking one large network and cutting it down into multiple smaller, manageable sub-networks.

### Analogy: Office Department Allocation

Instead of placing 200 workers in one giant, noisy hall, a company builds walls to separate departments into smaller rooms:

* **Marketing (MRK):** 100 devices
* **Sales:** 50 devices
* **Finance:** 30 devices
* **HR:** 10 devices

---

## 4. Subnetting Classifications

### 1. CIDR (Classless Inter-Domain Routing)

* Subnetting based on a **given bit size** allocated by a large network.
* **Key Formula:** `Given Bit = CIDR Bit = Network Bit`

#### CIDR Bit Ranges by Class:

* **Class A Range:** `/8` to `/15`
* **Class B Range:** `/16` to `/23`
* **Class C Range:** `/24` to `/30`

### 2. VLSM (Variable Length Subnet Mask)

* Subnetting tailored dynamically **as per specific requirement** (e.g., exact host counts per department).

---

## 5. Types of Subnetting

### A. Classful Subnetting

Occurs when the **Network Address** and the **Subnet Mask** belong to the **same class**:

| IP Address | Subnet Mask | Class Match |
| --- | --- | --- |
| `10.0.0.0 /8` | `255.0.0.0` | **Class A IP + Class A Mask** |
| `172.16.0.0 /16` | `255.255.0.0` | **Class B IP + Class B Mask** |
| `192.168.0.0 /24` | `255.255.255.0` | **Class C IP + Class C Mask** |

---

### B. Classless Subnetting

Occurs when the **Network Address** and the **Subnet Mask** belong to **different classes**:

| IP Address | Subnet Mask | Class Match |
| --- | --- | --- |
| `10.0.0.0 /16` | `255.255.0.0` | **Class A IP + Class B Mask** |
| `10.0.0.0 /24` | `255.255.255.0` | **Class A IP + Class C Mask** |
| `172.16.0.0 /24` | `255.255.255.0` | **Class B IP + Class C Mask** |

---

## 6. Subnetting Permission Rules

### Allowed Values (Valid Subnetting)

You can borrow host bits to create smaller networks:

* **Class A Network:** Can use `/8`, `/16`, or `/24` CIDR masks.
* **Class B Network:** Can use `/16` or `/24` CIDR masks.

### Not Allowed Values (Invalid Subnetting)

You cannot reduce default network bits:

* **Class C Network:** Cannot use `/8` or `/16` (cannot shrink the default 24 network bits).
* **Class B Network:** Cannot use `/8` (cannot shrink the default 16 network bits).

```

```
