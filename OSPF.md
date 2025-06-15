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

