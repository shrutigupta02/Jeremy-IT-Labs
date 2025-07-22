### **Default Gateway Limitations**

- A **default gateway failure** (router/interface) results in the **isolation of all hosts** relying on it, as they are configured with only **one static default gateway**.
    
- Even if an alternate router exists on the same VLAN, **clients cannot dynamically switch** to another gateway.
    
- In a **switched network**, clients can only use **one gateway IP**, and changes in topology don’t update the gateway automatically.

### **Router Redundancy**

- To avoid a **single point of failure**, networks use a **virtual router** setup using **First Hop Redundancy Protocols (FHRPs)**.
    
- **Multiple routers** share a **virtual IP and MAC address**, creating the illusion of a **single router** to host devices.
    
- **Host default gateway** is the virtual router IP. Host uses **ARP** to resolve the virtual IP to a **virtual MAC address**.
    
- The **active (forwarding) router** physically processes the traffic but is **transparent to the host**.
    
- A **redundancy protocol**:
    
    - Chooses the **active** router.
        
    - Manages the **failover** to a standby router if the active one fails.
        
- This seamless transition is called **first-hop redundancy**, allowing the network to **recover dynamically** without disrupting clients.


### **Steps for Router Failover**

When the **active router fails**:

1. The **standby router** stops receiving **Hello messages** from the active router.
    
2. The **standby becomes the new forwarding router**.
    
3. It takes over both the **IPv4 and MAC addresses** of the virtual router.
    
4. Hosts **experience no disruption** in connectivity, as they continue to send traffic to the virtual router address.

## Types:
**HSRP**
hot standby router protocol 
hsrp v6 - for ipv6

**GLB**: cisco prop, same as hsrp but allows load balancing between multiple redundant router
glb v6

**VRRP** -> virtual router redundancy prot, not cisco prop

## HSRP:
Within the HSRP group:

- The **active router** forwards traffic.
    
- The **standby router** monitors the active and takes over if the active router fails or meets specific failure conditions.

#### **HSRP Election Process**

- By default, the router with the **highest IPv4 address** becomes the **active router**.
    
- To **control router roles explicitly**, HSRP uses **priority values**.
    

#### **HSRP Priority**

- Configured using the `standby priority` command.
    
- Priority range: **0 to 255**; default: **100**.
    
- The **router with the highest priority** becomes the active router.
    
- If priorities are equal, **IPv4 address is used** as a tie-breaker (higher address wins).

#### **HSRP Preemption**

- By default, once a router becomes active, it **retains the role** even if a higher priority router comes online.
- **Preemption** allows a higher priority router to **take over as active** when it becomes available again.
- Enabled using the `standby preempt` command.
- **Preemption rules**:
    - A higher **priority** router can preempt an active router.
    - A router with equal priority but **higher IPv4 address** **cannot preempt**.
    - If **preemption is disabled**, the **first booted router** becomes active
### States:
1. initial: when intially config or interface first becomes available
2. learn: router has not yet received a hello packet and not yet gotten a virtual IP address
3. listen: got virtual IP, but the router is neither standby nor active, so it just listens the receiving hello packets
4. speak: participating in active/standby election
5. standby: alternate gateway / backup active
6. active: gateway

