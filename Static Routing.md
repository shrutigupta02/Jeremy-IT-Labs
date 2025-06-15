
- Static routes are manually configured by a network administrator.
- They explicitly define the path a packet should take to reach a specific destination network.
- Unlike dynamic routing protocols (like OSPF), static routes do not automatically adapt to network changes.

**Components of a Static Route**

- **Destination Network**: The IP network you want to reach (e.g., 192.168.2.0/24).
- **Next Hop**: The IP address of the next router in the path to the destination network, or the exit interface if directly connected.

**Configuring Static Routes on Cisco Routers**

- The command used is `ip route [destination network] [subnet mask] [next hop IP address or exit interface]`.
- Directly specified static routes: Instead of specifying a next-hop IP, you can specify the exit interface on the router.
- Fully Specified Static Routes: Include both the exit interface and the next-hop IP address. 
  `ip route 192.168.2.0 255.255.255.0 GigabitEthernet0/1 192.168.1.2`

- **Default Routes** (S*)
    - A special type of static route used to direct the traffic to the internet when no matching route is found in IP table.
    - Destination network is `0.0.0.0/0`, meaning "any network."
    - Example: `ip route 0.0.0.0 0.0.0.0 192.168.1.2`
    - aka the gateway of last resort

- **Administrative Distance (AD)**
    - A value that indicates the trustworthiness of a route. Lower AD is preferred.

