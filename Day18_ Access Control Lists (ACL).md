# Access Control Lists (ACL) - Complete Study Notes

---

## 1. What is an ACL?
* An Access Control List (ACL) is a list used to control network traffic by monitoring and filtering incoming and outgoing packets[cite: 13].

---

## 2. Types of Access Control Lists

### A. Standard ACL
* **Placement:** It is always used closest to the destination interface (FastEthernet, Ethernet, GigabitEthernet, or Serial)[cite: 13].
* **Inspection Criteria:** It always checks **only the Source Address**[cite: 13].
* **Processing Order:** The router performs the **Routing table lookup first**, and then applies the ACL[cite: 13].
* **Communication:** It is always used across two-way communication paths[cite: 13].
* **ACL ID Ranges:** Standard range is **1 – 99**, and the Advanced range is **1300 – 1999**[cite: 13].

### B. Extended ACL
* **Placement:** It is always used closest to the source interface (FastEthernet, Ethernet, GigabitEthernet, or Serial)[cite: 13].
* **Inspection Criteria:** It always checks **both Source & Destination Addresses**[cite: 13].
* **Deep Inspection:** It also checks specific **Protocols & Port Numbers**[cite: 13].
* **Processing Order:** The router performs the **ACL check first**, and then performs the Routing table lookup[cite: 13].
* **ACL ID Ranges:** Standard range is **100 – 199**, and the Advanced range is **2000 – 2699**[cite: 13].

---

## 3. Traffic Directions (2-Way Filtering)
* **Inbound (`in`):** A packet that is coming inside a router interface[cite: 13].
* **Outbound (`out`):** A packet that is going outside a router interface[cite: 13].

---

## 4. How ACL Works (Core Processing Rules)
1. **Top-to-Bottom Evaluation:** Rules are read sequentially from top to bottom[cite: 13].
2. **Stop on First Match:** Evaluation stops immediately upon the first matching rule[cite: 13].
3. **Implicit Deny:** Every ACL contains an **invisible implicit deny** statement at the very end, which drops any traffic that hasn't matched a preceding rule[cite: 13].
4. **Directional Enforcement:** ACLs must be explicitly applied in a specific direction (Inbound or Outbound) on an interface[cite: 13].

---

## 5. Example Reference: ACL-11 Rule Evaluation Table

| IP Address / Target | Action | Description |
| :--- | :---: | :--- |
| `10.0.0.10` | **D** | Deny packet matching this source[cite: 13] |
| `192.168.0.1` | **D** | Deny packet matching this source[cite: 13] |
| `172.16.0.11` | **D** | Deny packet matching this source[cite: 13] |
| `10.0.0.1` | **P** | Permit packet matching this source[cite: 13] |
| `192.168.0.100` | **P** | Permit packet matching this source[cite: 13] |
| `20.0.0.1` | **P** | Permit packet matching this source[cite: 13] |
| *[End of List]* | **Deny** | **Invisible implicit deny** drops all other unmatched traffic[cite: 13] |

```
