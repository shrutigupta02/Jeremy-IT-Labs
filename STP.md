Redundancy is important for network design to ensure fault tolerance.

**The Problem of Layer 2 Loops:** In redundant switched networks, broadcast frames can loop infinitely, causing broadcast storms and MAC address flapping (continuous lookup and updation of mac add table on each switch)
## Spanning Tree Protocol: 802.1D
Switches from all vendors run STP by default.
#### How STP Solves Loops:
STP is a Layer 2 protocol that prevents these loops by putting the redundant ports *(matlab jo extra paths hote hai na that connect hosts to network in case one link breaks)* in a blocked state and these serve as backup which only come into active state when the current link breaks.

Interfaces in blocking states only send and receive BPDU: bridge protocol data units (stp messages)

**By selecting which ports are forwarding and which are blocking, STP creates a single active path from one end to another.**

STP-enabled switches send/receive ***Hello BPDUs out of all interfaces, once every 2 seconds.***
If a switch receives a Hello BPDU on an interface, it knows that interface is connected to another switch.

Switches use a field in BPDU called Bridge ID used to elect a root bridge for the network.
The switch with the lowest bridge id becomes the root bridge.
Bridge ID:  has MAC address and Bridge priority
Bridge priority: has 4 bits of bridge priority and 12 bits of VLAN id (called extended system id)

- **Root Bridge:** all interfaces on rb are put in a forwarding state and every switch on the network must have a path to reach the RB.

- **Root Port:** Each non-root switch selects one port as its root port, which is the port with the **best path** to the root bridge. This port is also in a "forwarding" state. Port across of root port is assigned designated port.

When the switch is powered on, every switch assumes they are the root bridge. Hence all switches send BPDUs.
It will give up the position when it receives the BPDU of lower bridge ID.
Once network is converged, every switch will agree on a single root bridge and only it will send BPDUs.

##### STP Cost:
| Speed of interface | Cost | RSTP    |
| ------------------ | ---- | ------- |
| 10mbps             | 100  | 2000000 |
| 100mbps            | 19   | 200000  |
| 1gbps              | 4    | 20000   |
| 10gbps             | 2    | 2000    |
- cost of all interfaces on root bridge will be 0.
- if a switch has more than one interface with the lowest total cost then the interface connecting to the neighbour with the lowest bridge id is selected.
- if that is also same then we'll consider lower neighbour port id
port id = port priority + port number
- g0/0 < g0/1 < g1/0 < g1/1

it can be viewed by:
`show spanning-tree` 
`show spanning-tree vlan 1`

#### Choosing Blocking port:
- every connection (aka collision domain) needs one Designated port (forwarding port) and either one root port or one blocking port. 
- how to choose blocking port?
- switch with lowest root cost will have designated port and switch with higher cost will make blocking port.
- if both are same then switch with higher bridge id will make blocking.

### PVST
Per-VLAN Spanning Tree (PVST) versions of STP, there is a root bridge elected for each spanning tree instance. This makes it possible to have different root bridges for different sets of VLANs. STP operates a separate instance of STP for each individual VLAN. If all ports on all switches are members of VLAN 1, then there is only one spanning tree instance.

### STP Port States:
1. **Blocking**:
	- can receive frames
	- cannot forward
	- cannot learn MAC addresses
2. **Forwarding**
3. **Listening**: Transitional state. After blocking state, port enters this state.
	- 15 seconds by default
	- can only send and rec STP BPDUs
	- cannot send and rec regular traffic
	- does not learn mac addresses from regular traffic
4. **Learning**: Transitional state. After listening state, 
	- 15 seconds
	- similar to listening state it can only send and rec stp BPDUs and not regular traffic
	- **LEARNS** mac addresses from regular traffic
5. **Disabled:** does not participate in stp

### STP Timers:
1. **Hello: 2 secs**. How often will root send BPDUs.
2. **Forward Delay: 15 seconds.** Transitional time for listening and learning states.
3. **Max Age: 20 seconds**. How long will an interface wait after not recieving hello packets for.

### STP Toolkit:
1. **Portfast:** It allows ports to move to forwarding state  immediately, bypassing listening and learning states. **Only to be used on switches connected to end hosts**, otherwise it will cause a layer 2 loop.
   ```
   interface f0/0
   spanning-tree portfast
	```

2. **BPDU Guard**:  basically any portfast interface should never receive a BPDU packet because that would mean that the portfast interface is connected to another switch. Hence bpdu guard is a cisco feature where as soon as a portfast interface receives a bpdu message, it disables the port to prevent infinite loops.
```
interface f0/0
spanning-tree bpduguard enable
```

3. **Root Guard:** basically helps maintain the spanning tree topology, if someone plugs a lower bridge id switch, the root bridge would not be automatically changed.
4. **Loop guard:** basically when enabled on an interface, even if it stops receiving bpdus it will not start forwarding them, prevents loops in case of unidirectional failures.

##### Setting a switch as a primary or secondary root for a vlan:
```
SW1(config)# spanning-tree vlan 10 root primary
SW1(config)# spanning-tree vlan 20 root secondary
```

## RSTP: Rapid Spanning Tree Protocol

#### What is RSTP?
- **RSTP** stands for **Rapid Spanning Tree Protocol** and is defined by **IEEE 802.1w**
#### Same Algorithm, Faster Response
- RSTP uses the **same spanning tree algorithm** as STP to select root bridges, root ports, and block loops.
- The **key difference**: **much faster convergence**.
    - STP can take **30 to 50 seconds** to reconverge.
    - RSTP can do this in **less than a second** (hundreds of milliseconds).
##### Fast Convergence Example
- RSTP introduces **alternate ports**, which can **immediately start forwarding traffic** if the primary path fails—**no need to wait for timers**.

>[!note]
>Cisco’s implementation of RSTP is called Rapid-PVST+ (Per VLAN Spanning Tree+).
>This means **each VLAN** has its **own RSTP instance**, improving efficiency and fault isolation.
    
| **STP (802.1D)** | **RSTP (802.1w)** |
| ---------------- | ----------------- |
| Disabled         | Discarding        |
| Blocking         | Discarding        |
| Listening        | Discarding        |
| Learning         | Learning          |
| Forwarding       | Forwarding        |
1. **Discarding**: Merges the 3 non-forwarding states—**no data is forwarded**, no MAC addresses learned.
2. **Learning**: Port learns MAC addresses but doesn’t forward traffic.
3. **Forwarding**: Port sends and receives all network traffic.
#### Port Roles:
| **Port Role**            | **Explanation**                                                                               |
| ------------------------ | --------------------------------------------------------------------------------------------- |
| **Root Port (RP)**       | The port with the **best path to the root bridge**. Only one per switch.                      |
| **Designated Port (DP)** | **Forwards traffic** away from the root bridge towards other segments. One per segment.       |
| **Alternate Port**       | A **backup port** to the root port. If the root port fails, this can take over **instantly**. |
| **Backup Port**          | A **redundant backup** to a designated port on a **shared segment** (like with a hub).        |
#### Key Difference from STP:
- In **STP**, **blocked** ports just sit idle.
- In **RSTP**, **alternate** and **backup** ports are **actively ready** to transition to forwarding if needed, greatly speeding up recovery.