# Day 3: The OSI Reference Model & Data Encapsulation

## 1. OSI Reference Model
* **OSI:** Open Systems Interconnection reference model.
* Developed by **ISO** (International Organization for Standardization) in **1983** to standardise network architecture.
* Divided into **7 Functional Layers**:

| Layer # | Layer Name | Layer Grouping | Layer Mnemonic Devices |
| :---: | :--- | :--- | :--- |
| **7** | **Application** | Upper / Software Layer | **A**ll / **A**ndhra |
| **6** | **Presentation** | Upper / Software Layer | **P**eople / **P**radeesh |
| **5** | **Session** | Upper / Software Layer | **S**eems / **S**e |
| **4** | **Transport** | Middle Layer | **T**o / **T**rain |
| **3** | **Network** | Lower / Hardware Layer | **N**eed / **N**ew |
| **2** | **Data Link** | Lower / Hardware Layer | **D**ata / **D**elhi |
| **1** | **Physical** | Lower / Hardware Layer | **P**rocessing / **P**ahuchi |

---

## 2. Encapsulation & Protocol Data Units (PDU)

### Encapsulation Flow
1. **Application Layer:** Plain Text Data.
2. **Presentation Layer:** Encryption / Formatting (Cipher Text).
3. **Session Layer:** Session Headers / Timers added.
4. **Transport Layer:** Port numbers and Transport headers added (Segments).
5. **Network Layer:** Source & Destination IP addresses added (Packets).
6. **Data Link Layer:** Source & Destination MAC addresses added (Frames).
7. **Physical Layer:** Binary streams ($0\text{s}$ and $1\text{s}$) transmitted across physical media.

### Summary Table of Layers, PDUs, and Hardware
| OSI Layer | PDU Name | Information / Headers Added | Associated Devices |
| :--- | :--- | :--- | :--- |
| **7. Application** | Data | Plain text | End User PC / Application |
| **6. Presentation** | Data | Cipher text / Compression | End User PC |
| **5. Session** | Data | Timers / Session ID | End User PC |
| **4. Transport** | Segment | Source & Destination Ports | End User PC / Firewalls |
| **3. Network** | Packet | Source IP & Destination IP | Routers, Layer 3 Switches |
| **2. Data Link** | Frame | Source MAC & Destination MAC | Layer 2 Switches, Bridges, NICs, WICs |
| **1. Physical** | Bit | Binary Data Stream (`0101...`) | Hubs, Repeaters, Amplifiers, Cables |
