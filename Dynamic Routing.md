**Network route**: any route to a subnet or network, mark length < = 30
**Host route**: any route to a single host, mask length /32

**Benefits of Dynamic Routing**:

- **Automatic Updates**: Routers automatically inform each other about new destination networks.
- **Path Redundancy and Failover**: if one route/link fails, it automatically switches to the other working link.

##### How does dynamic routing work:

- Routers advertise information about their connected routes and learned routes.
- They form **adjacencies** (neighbour relationships) with adjacent routers to exchange info.
- If multiple routes to a destination are learned, the router uses the **metric** to determine the superior route (lower metric is better)

### Types of dynamic routing:
1. **IGP**: Interior Gateway Protocol. Used to share routes within a single **Autonomous System (AS)** (single organization).
   Example: RIP, EIGRP, OSPF
2. **EGP**: Exterior Gateway Protocol. Used to share routes between different autonomous systems.
   Example: BGP (Path vector: sharing routes betweem AS)
   
##### Algo types:
1. **Path Vector**
2. **Distance Vector**:
	- Send known destination networks and their metrics to directly connected neighbors. Often called **"routing by rumor"** because routers only know what their neighbors tell them, not the full network map. They learn the "distance" (metric) and "vector" (direction/next hop)
2. **Link State**:
	- Every router creates a **complete connectivity map** of the network, which is the same on all routers. They advertise information about their interfaces and connected networks, allowing all routers to develop the same map. They use more CPU/memory but react faster to network changes

![[Screenshot 2025-06-15 at 7.34.49 PM.png]]
###### Routing Metrics:
**Equal Cost Multi-Path (ECMP) Load Balancing** : If a router learns two or more routes to the same destination via the same routing protocol and with the same metric, both will be added to the routing table, and traffic will be load balanced over them. This also applies to static routes with a metric of 0

1. RIP: hop count
2. EIGRP: bandwidth and delay
3. OSPF: cost, calculated based on bandwidth
4. IS-IS: cost

**Administrative Distance**: Used to determine which routing protocol is preferred when a router learns two different routes to the same destination via _different_ routing protocols.

| Routes             | AD Value |
| ------------------ | -------- |
| Directly Connected | 0        |
| Static             | 1        |
| BGP                | 20       |
| EIGRP              | 90       |
| OSPF               | 110      |
| IS-IS              | 115      |
| RIP                | 120      |
| EIGRP (External)   | 170      |
| Unusable           | 255      |
**Configure AD for a static route:**
```
ip route {destination network} {subnet mask} {next hop} <1-255
```

###### Floating Static routes:
Changing the AD of a static route to make it less preferred.

