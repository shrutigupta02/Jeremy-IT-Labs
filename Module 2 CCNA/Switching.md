switching anf forwarding of frames.
two ports:
1. ingress: entry to switch
2. egress: exit

LAN uses switching table to forward traffic based on:
1. ingress port
2. destination MAC

MAC table is stored in **CAM (Content Addressable Memory)**, optimized for fast lookups. Thus, it's also called the **CAM table**.

## Switch learn and forward:
#### Step 1: **Learn** – Examine **source MAC address**

- If the source MAC is **not in the MAC table**, it is **added** with its ingress port.
- If it **exists**, the **refresh timer is updated**.
- If it exists but on a different port, the **entry is replaced** with the new port.

#### Step 2: **Forward** – Examine **destination MAC address**

- If the destination MAC is **in the table**, forward the frame to that port.
- If not in the table:
    - **Flood** the frame out **all ports** except the incoming one (called **unknown unicast**).
- If it’s a **broadcast or multicast**, also **flood** it out all ports except ingress.

### **Switching Forwarding Methods**

- **Switches make Layer 2 decisions fast** using **ASICs (Application-Specific Integrated Circuits)**.
- Two Layer 2 switching methods:
	- **Store-and-Forward Switching**
	- **Cut-Through Switching**

#### Store and forward:
- used by cisco
- error checking and buffering

#### Cut through:
- faster
- Begins forwarding as soon as **destination MAC address is read**, **without waiting** for the entire frame or verifying FCS.
- Suitable for **high-performance computing (HPC)** environments that demand **<10 microsecond latency**.

##### Fragment-Free Switching
- Modified cut-through method.
- Forwards frame only after the first **64 bytes**
---

A **collision domain** is a network segment where **data collisions** can occur if two or more devices transmit at the same time.

A **broadcast domain** includes all devices that can **receive a broadcast frame** sent from a device.

---

# VLANS
### benefits of vlans:
1. smaller broadcast domains.
2. improved security
3. improved efficiency 
4. simpler management 

### types:
1. default: 1
	- All ports are assigned to VLAN 1 by default.
	- The native VLAN is VLAN 1 by default.
	- The management VLAN is VLAN 1 by default.
	- VLAN 1 cannot be renamed or deleted.
2. data vlan:
	- Carries **user-generated traffic** (emails, web access, file transfers, etc.).
	- Should not carry management or voice traffic.
  3. native
	- Used in **802.1Q trunking** to carry untagged traffic.
  4. management: Used to **access and manage network devices** (e.g., switch GUI, SSH).
  5. voice: Supports **VoIP (Voice over IP)**. supports qos by separating traffic.

### Trunk:
A **trunk** is a **point-to-point** link that carries multiple VLANs. It uses the **IEEE 802.1Q** standard and is not associated with a single VLAN.

**VLAN trunks** allow **multiple VLANs** to pass traffic between switches, enabling devices in the same VLAN but on different switches to communicate.

### voice vlan:
A **Cisco IP phone** has an integrated **3-port switch**:

- Port 1: Connects to switch or another VoIP device.
- Port 2: Internal for IP phone’s traffic.
- Port 3: Connects to a **PC or host**.

` S1# show interfaces fa0/18 switchport `

## Vlan ranges:
1 and 1002-1005: legacy tech
 - stores in vlan.dat in flash memo
 - support VTP

1006-4094:
- **fewer features** than normal VLANs
- vtp transparent mode ony


 