## Binary to Hex:
groups of 4 bits -> decimal -> hex.
ex: 11011011 -> 1101 & 1011 -> 13 & 11 -> DB

## Why IPv6?

- not enough IP addresses.
- ipv4 -> 2^(32) addresses around 4 billion

- IPv6 has - 340,282,366,920,938,463,463,374,607,431,768,211,456 addresses
- written in hexadecimal
- **32 hexadecimal characters**
- divided in 8 groups of four hex digits 

#### Shortening IPv6 addresses:

1. remove aage waale zeroes
2. consecutive groups of zeroes can be removed: 
	- ex: 08DD : 2008 : 0000 : 0000 : 0000 : 0000 : 100B : CA00
	- converted to 8DD : 2008 : : 100B : CA00
	- however, if there are more than one group of consecutive groups of zeroes:
	- like: 2008 : 0000 : 0000 : BB10 : 0000 : 0000 : 0000 : CA32
	- converted to -> 2008 : 0 : 0 : BB10 : : CA32

### IPv6 prefixes:
- usually enterprises get /48 and usual networks use /64 hence rest 16 bits are used to make subnets.

Example IPv6 range: 2001 : 0db8 ::

### Configuring IPv6:
```
ipv6 unicast-routing
interface g0/0
ipv6 address <address along side prefix /x >
no shutdown
```

**Show Commands:**

```
show ipv6 interface brief
```

## EUI - 64

**Extended Unique Identifier**:
Method of converting a 48bit MAC address into a 64 bit interface identifier.

This interface identifier can then become the host portion of a /64 IPv6 address.

#### Steps:
1. divide mac address into two halves

2. insert FF in the end and beginning of the halves

3. invert the 7th bit

#### Configure EUI-64 addresses on router:
```
interface g0/0
ipv6 address <address network bits eg:> 2001:08db::/64 eui-64
no shutdown
```

### Global Unicast Addresses:
public ipv6 addresses which can be used over the internet.

defined from: 
2000 :: / 3 to 3FFF : FFFF : FFFF : FFFF : FFFF : FFFF : FFFF : FFFF

These were originally allowed but now all ipv6 addresses that arent reserved for any other puposes are gloabblly unicast addresses.

### Unique Local Addresses:
not globally unique

free to use

cannot be used over the internet 

kind of like private ipv4 addresses (10.10.10.10)

begin with FD

### Link Local Addresses:
automatically generated on ipv6 enabled interfaces

`ipv6 enable` -> to enable them

start with FE

used for communication within a single link. cannot be used to route by routers.

**Why use Link Local Addresses?**
- NDP - neighbour discovery protocol (ARP for ipv6) uses LLA
- OSPF uses LLA for adjacency 
- next hop for static routes use LLA

### Multicast addresses in IPv6 and IPv4:

![[Screenshot 2025-06-17 at 7.18.07 PM.png]]
 -> IPv6 doesnt have broadcast

#### IPv6 Multicast Scopes:
1. Interface local (FF01) - stays within a single device, doesnt leave the interface.
2. Link Local (FF02)- stays within the subnet
3. Site local (FF05) - can leave the subnet but should stay within a single physical location, does not travel between WAN
4. Organisation Local (FF08) - wider scope than site local
5. Global (FF0E) - can be routed to internet

### Anycast:
new concept used in ipv6

**One to One of Many**

Working:
- multiple routers are configured with the same ipv6 addresses
- when a packet is destined for an anycast address, it is sent to the nearest router with that address.

`ipv6 address <address> anycast`

## Other IPv6 addresses:
- unspecified- ::/0
- loopback- ::1

## IPv6 Header:
![[Screenshot 2025-06-17 at 8.39.52 PM.png]]

**Its fixed length: 40bytes**

Version: ipv4 or 6
Traffic class: Used for QoS
Flow Label: identify specific traffic flows
Payload length: length of transport layer segment
Next header: TCP or UDP
Hop limit: TTL

### Solicited Node Multicast address

Calculated from unicast address:
ff02 : 0 : 0 : 0 : 0 : 0 : 01 : ff + **last 6 digits of unicast address**
This is used by NDP

#### NDP - Neighbour Discovery Protocol:
the arp of ipv6

uses ICMPv6 and Solicited Node Multicast Addresses to build mac address tables.

the types of messages used:
1. NS - neighbour solicitation mesage - ICMPv6 type 135 (ARP req)
2. NA - neighbour advertisement - ICMPv6 type 136 (ARP reply)

`show ipv6 neighbour table` -> view MAC address table

**Another function of NDP:** allows hosts to automatically discover routers on the network.

Two messages: 
1. RS - Router Solicitation - ICMPv6 type 133
	- sent to FF02::2 -> all routers
	- asks them to identify themselves
	- sent when interface is enabled or host is connected to network
2. Router Ad - ICMPv6 type 134
	- sent to FF02::1 -> all nodes
	- sent either in response to RS or periodically

---
## SLAAC: Stateless Address Auto-Config

kind of like DHCP of ipv6
- hosts use RS and RA to learn the ipv6 prefixes of the local link and then generate ipv6 address themselves.
- `ipv6 address autoconfig`
- using this command we do not need to manually enter the ipv6 address.
- the device will use EUI-64 to generate an interface id

## DAD - Duplicate Address Detection

- allows host to detect if other hosts on the network are using the same address
- anytime an ipv6 interface is enabled using no shutdown, or address is configured using manual, slaac etc, DAD is performed
- DAD uses NS and NA
- hosts sends a NS to its own address, if it doesnt get a reply then great but if it does get a reply then it means other host is using this address.

## IPv6 Static Routing:
to enable: `ipv6 unicast-routing`

by default ipv6 routing table looks exactly like ipv4:
- network routes - C
- local routes - L
- Default routes - use ::/0 equivalent of 0.0.0.0/0 in IPv4

```
ipv6 <destination address with prefix> {next hop OR exit-interface } [AD]
```

1. Directly connected: **ONLY WORKS WITH SERIAL OR OTHER NON-ETHERNET INTERFACES**
	- ipv6 destinationIP interface
2. Recursive:
	- ipv6 destination nexthop
3. Fully specified:
	- ipv6 destination interface nexthop
