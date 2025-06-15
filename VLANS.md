**Why do we need VLANS?**
-> To divide a LAN further into smaller networks, but subnetting is a layer 3 protocol and layer 2 protocols like broadcasting do no adhere to subnets if they are all connected to the same gateway.
-> Example: a comapny has three blocks: science, sales, marketing. All connected to the same gateway hence one single LAN. But they're all in different subnets. Still, if science block broadcasts something to all hosts of science block only, using the broadcasting address of science block's subnets, still the message would be broadcasted to every single host in marketing and sales block as well. This is a security risk and an overhead issue as well.
A solution to this can be to buy separate switches for each block but it isnt cost effective.

Hence we create VLANs. All vlans connected to the same gateway through one single switch can broadcast and send unknown unicast frames between hosts of the VLAN only.

Switch doesn't perform inter-VLAN routing, any message needed to be sent to a host in another vlan has to go through a router.

***Subnets separate networks at layer 3***
***VLANS logically separate networks at layer 2***

`show vlan brief` -> displays vlans info

Total number of possible VLANs: 2^12 = 4096
Non usable VLANs -> 1, 1002-1005
Reserved VLANs -> 0, 4095

#### How to assign interfaces to a VLAN:
```
interface range g1/0 - 3
switchport mode access
switchport access vlan 10
```

here we assigned the interface g1 g2 g3 to vlan 10.

switchport mode access -> can configure access port i.e. a port on a switch that belongs to a single vlan. access port aka untagged port.

#### Trunk Ports
Used to carry traffic from multiple VLANs on a single interface. aka tagged port.
##### Assign a switch interface as trunk:
```
interface fa0/8
switch mode trunk
exit
```

![[Screenshot 2025-06-13 at 6.25.01 PM.png]]

802..1Q is the tagged field that indicates to the switch 2 what VLAN its directing traffic from/into.
It’s a tag added to each message (or data packet) when it travels through the trunk line.

![[Screenshot 2025-06-13 at 8.33.46 PM.png]]

### Router on a Stick:
list of all commands:
```
ROUTER CLI
config t
interface g0/0 (whatever interface on router)
no shutdown
interface g0/0/0.10 {.x value is recommended to match w vlan number}
encapsulation dot1q 10 (10 is vlan number)
ip address 192.168.10.4 255.255.255.0 (assign the last usable address of the subnet)
```

### Native VLANs:
basically vlans whose traffic you can send untagged on trunk ports. by default 1.

two ways to set a native vlan:
`switchport trunk native vlan <vlan-id>`

```
encapsulation dot1q <vlan-id eg 10> native
ip address {address} {subnet mask}
```

```
interface g0/0
ip address {address} {subnet mask}
```

## Layer 2 switch : Multilayer switch
- capable of switching and routing both.
- can assign ip addresses to its interfaces like a router and unlike L2 switches.
- can be used for inter vlan routing by itself i.e. it doesnt send traffic to router to perform inter vlan routing.
- have SVI - switch virtual interfaces. 

SVI: these are logical interfaces you can assign ip addresses to in a L3 switch.
Each SVI is associated with a specific VLAN and can be assigned an IP address. This allows traffic to be routed between VLANs, as each VLAN essentially has its own logical interface and broadcast domain.

### Commands for intervlan routing using SVI:
```
ip routing
interface g0/1
no switchport
ip address {address} {subnet mask}
exit
```

`ip route 0.0.0.0 0.0.0.0 {address of router}`

```
interface vlan 10
ip address {any unused address of vlan} {subnet mask}
no shutdown
```

### Conditions required for status to be up - up on interfaces table:
1. VLAN must exist on the switch
2. the switch must have an access port in the vlan with an up up state or a trunk port with an up up state
3. the vlan must be no shutdown
4. the SVI must be no shutdown

## DTP: Dynamic Trunking Protocol
Cisco proprietary protocol that allows cisco switches to determine their interface status (access or trunk) without manual config that means we do not need to use commands like switchport mode access or switchport mode turnk.
DTP can be exploited by attackers hence manual config is best and dtp is not used.

### DTP Configuration Modes:

- `switchport mode dynamic desirable`: **Actively tries to form a trunk** with other Cisco switches. It will form a trunk if connected to a port in trunk, dynamic desirable, or dynamic auto mode. old switches were default set to desirable
- `switchport mode dynamic auto`: Does not actively try to form a trunk. It will form a **trunk only if the connected switch is actively trying to form one**. new switches are default set to auto.

**Disabling DTP**:

- `switchport nonegotiate` command stops the interface from sending DTP negotiation frames 
- Configuring an access port with `switchport mode access` also disables DTP negotiation

**Trunk Encapsulation Negotiation**: 
Switches supporting both Dot1Q and ISL trunk encapsulations can use DTP to negotiate the encapsulation type. ISL is favoured over Dot1Q if both are supported.

## VTP: VLAN Trunking Protocol
Allows you to configure VLANs on one central switch known as VTP Server and other switches (VTP clients) can copy the config of vlans on them so you do not have to configure vlans on each switch.

Versions: 1, 2, 3
Modes: Server (default), Client, Transparent

#### VTP Servers:
- can add/modify/delete vlans
- store vlan db in nvram
- increase revision number everytime a new vlan is added/mod/del
- advertise the new versions of vlan db to all vtp clients
- can also serve as vtp clients to a vtp server with a higher revision number

#### VTP Clients:
- sync their db with vtp server of highest revision number
-  advertise the new versions of vlan db to all vtp clients 

`show vtp status`

 `vtp domain cisco`

##### One danger of VTP:
If you connect an old switch with a higher revision number to your network (and the VTP domain name matches), all the switches in the domain will sync their VLAN database to that older config switch.

#### VLAN Transparent mode:

- Does not participate in the VTP domain (does not sync its VLAN database).
- Maintains its own VLAN database in NVRAM. It can add/modify/delete VLANs, but they won't be advertised to other switches.
- Will **forward** VTP advertisements that are in the same domain as itself.

```
vtp mode client / transparent
```

```
vtp version 2 / 3
```