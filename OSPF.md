#### Open Shortest Path First:

uses dijkstra's algorithm

three version:
- OSPFv1
- OSPFv2 -> used for ipv4
- OSPFv3 -> used for ipv6

**Routers store info about network in LSA - Link State Advertisements and organised in LSDB - Link State DB**

Routers in the same network will **flood LSAs** until all the routers build the same connectivity map.

**LSA Aging Timer**: Each LSA has a default aging timer of 30 minutes, after which it is flooded again.

##### Three Main OSPF Steps:

1. **Become Neighbours**: Routers connect and establish neighbor relationships with others on the same network segment.
2. **Exchange LSAs**: Neighbors exchange LSAs to build their LSDBs.
3. **Calculate Best Routes**: Each router independently uses the SPF algorithm to calculate and insert the best routes into its routing table.

### OSPF Areas

- **Purpose of Areas**: OSPF uses areas to divide large networks into smaller sections, which improves performance. Small networks can operate as a single area without issues.

- **An area is a set of routers which share the same LSDB.**

- **Negative Effects of Single-Area Design in Large Networks**:
    - SPF algorithm takes more time and processing power to calculate routes.
    - A single large LSDB consumes more router memory.
    - Every network change causes LSAs to be flooded to all routers, requiring repeated SPF calculations.
- **Area 0 (Backbone Area)**: all non backbones connect to this

##### OSPF Area Terminology:

- **Internal Routers**: Routers with **all interfaces** within the same area.
  
- **Area Border Routers (ABRs)**: Routers with interfaces in multiple areas. 
	- ABRs maintain a separate LSDB for each connected area
	- recommended to connect an ABR to a maximum of two areas.
	  
- **Backbone Routers**: Routers connected to Area 0 (includes ABRs connected to Area 0).
- **Intra-Area Route**: A route to a destination within the same OSPF area.
- **Inter-Area Route**: A route to a destination in a different OSPF area.

##### OSPF Rules:
- every area should be contiguous i.e. connected. example: should not be like pakistan and bangladesh before separate countries.
- all areas should have atleast one ABR connected to backbone area.
- ospf interfaces in the same subnet should be in the same area.

### Configuration:
```
router ospf 1
network <network id> <wildcard sm> area 0
```

**Wildcard Subnet Mask**: ulta legit bass ulta karde subnet mask ka

##### This configuration command tells router to:
1. look for interfaces on the router with an ip address in the specified range
2. activate ospf on those interfaces
3. look for OSPF activated neighbour routers to become ospf neighbours

`passive-interface g2/0` -> tells router to stop sending hello packets from that interface, however router still sends LSAs to other routers. **this command should be used on interfaces which are not connected to any routers**, because its pointless to send hello packets where no routers are present.

##### Router ID order of priority:

1. ﻿﻿﻿Manual configuration
2. ﻿﻿﻿Highest IP address on a loopback interface
3. ﻿﻿﻿Highest IP address on a physical interface

**to manually configure router id:**
`router-id 1.1.1.1`

**ASBR** - Autonomous System Border Router
- an ospf router that connectes ospf network to external network

`default-information originate` -> sets router as ASBR

---
### OSPF Cost Metric:
**Cost Calculation:** dividing the reference bandwidth with interface bandwidth. 
- default reference bandwidth is 100mbps

| Interface Type   | Cost |
| ---------------- | ---- |
| Ethernet         | 10   |
| Fast Ethernet    | 1    |
| Gigabit Ethernet | 1    |
| 10Gig Ethernet   | 1    |
OSPF cost min value = 1 so even when the calculation is less than 1 we write it aas 1.

`show ip ospf interface g0/1` -> to see the cost of interface

We should configure the reference bandwidth to be more:
`auto-cost reference-bandwidth 100000` -> to change reference bandwidth

- Note - loopback interfaces have a cost of 1.

##### Manually configure interface cost:
`ip ospf cost 1000`

this cost takes priority over calculated cost.

###### Another way: change the bandwidth value of the interface.
this does not change the interface bandwidth actually. just the value used for cost calculation 
Not reccomended to change cause it affects other things too.

`bandwidth 100000`

The actual method to change the bandwidth is using the speed command.

---
### OSPF Neighbours:

- When OSPF is activated on an interface, the router starts sending OSPF hello messages out of the interface at regular intervals (determined by the hello timer). These are used to introduce the router to potential OSPF neighbours.

- default hello timer - 10 seconds/

- multicast address for hello messages: 224.0.0.5

#### Stages of router while estsablishing neighbours:

1. **Down:** initial state, no hello packets received from neighbour
2. **Init**: Hello received from neighbour, but own router id not in hello packet yet
3. **Two-Way:** Both routers have received Hellos with their own Router IDs in the packet, now they are ospf neighbours. they'll maybe elect DR (designated router) and BDR (backup DR) at this stage.
4. **Exstart:** slave master
5. **Exchange**: exchange DBDs of a list of LSAs, compare the info to their own LSAs to see which ones they dont have
6. **Loading**: Routers send Link State Request (LSR) messages to request missing LSAs, which are then sent in Link State Update (LSU) messages which are acknowledged using LSAck messages.
7. **Full**: Routers have a full OSPF adjacency and identical LSDBs. They continue sending Hellos to maintain the adjacency, with a Dead Timer (default 40 seconds)
 
 ### OSPF commands:
 `show ip ospf neighbor`

---
## OSPF Network Types:

1. **Broadcast:** 
	- Routers dynamiccaly discover ospf neighbours using hello packets, one DR and BDR are elected and exchange of LSAs from DROthers happen via DR and BDR only.
	- The purpose of DR/BDR is to reduce LSA flooding on multi-access networks. DR Others only form full adjacencies with the DR and BDR, remaining in a two-way state with other DR Others.

2. **P2P**: routers dynamically discover neighbours using multicast address
	- No DR or BDR elections are held because these are direct connections between two routers.
	- The default encapsulation on Cisco serial interfaces is HDLC (specifically cHDLC), which can be changed to PPP using the `encapsulation ppp` command. Encapsulations must match on both ends of the connection.

3. **Non Broadcast :**  This is the default for Frame Relay and X.25 interfaces. It uses a default hello timer of 30 seconds and a dead timer of 120 seconds.

4. **Manually configuring network type**: The `ip ospf network [type]` command can be used, though not all network types work on all link types (e.g., serial links cannot use broadcast network type).

### OSPF neighbor and adjacency requirements:

- Area number must match
- Interfaces in same subnet
- Hello and dead timers must match
- OSPF network type must match
- OSPF router ID must be unique
- OSPF must not be shut down
- Authentication settings must match

## LSA (Link State Advertisement) types:

1. **Type 1 - Router LSA**:
	- generated by every router on ospf
	- has Router ID
	- lists neighbours on its OSPF interfaces
	  
2. **Type 2 - Network LSA**:
	- generated by the DR of every multi-access network i.e. broadcast network
	- has info of routers in network
3. **Type 5 - AS External LSA**:
	- generated by ASBR
	- describes routes to addresses outside OSPF AS, like default gateway

##### DR, BDR election criteria:

1. higher interface / port priority: 
	- `ip ospf priority <number>`
2. higher router id
