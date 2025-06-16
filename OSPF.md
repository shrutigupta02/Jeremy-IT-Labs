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

`show ip ospf interface g0/1`