#### Access and distribution switches:

Access -> a switch end hosts are connected to
Distribution -> a switch access switches are connected to 

**Oversubscription**: when the total bandwidth of the interfaces connected to the end hosts is greater than the bandwidth of the interfaces connected to the distribution switches.
Some oversubscription is okay but on a greater scale, it causes congestion.

### Increasing number of interfaces connected to the distribution switches:

![[Screenshot 2025-06-15 at 1.54.50 AM.png]]

Due to STP, only one interface remains in forwarding mode. Hence not a solution for congestion.

Etherchannel can solve this problem by giving active redundancy interfaces while preventing loops or broadcast storms.

### Ether Channel:

it logically groups all interfaces connecting access switches to distribution switches so all interfaces contribute to bandwidth increase but do not cause broadcast storms.

whenever the ASW1 receives a broadcast frame from any end host then it only forwards one copy to DSW1 instead of 4 copies for 4 interfaces.

ether channel happens only logically / virtually, hence it does not physically group interfaces.

other names:
- Port Channel
- LAG -> link aggregation group

#### Load Balancing:

when end hosts send a series of packets through ether channel, the switch sending traffic on it decides which physical interface will be used, and the same interface is used to transmit the frames from a particular end host otherwise frames might arrive out of order.

to decide which frames will be sent on what interface we consider the following inputs:
- MAC address: frames w same source mac addresses are sent on a single interface
- Destination mac
- Source and dest mac
- source ip
- dest ip
- source and dest ip

`show etherchannel load-balance`

to change the input for load balancing:
`port-channel load-balance src-dst-mac / ip`

### Method for configuring ether channels:

##### PAGP:
- port aggregation protocol
- cisco's proprietary 
- dynamically negotiates the creation and maintenance of ether channels
- similar to DTP for trunking.

###### Configuration:
```
interface range g0/0-3
channel-group 1 mode auto / desirable
```

**Channel Mode:**
1. ON: initiates ether channel without pagp
2. Auto: places an interface in a passive negotiating state. i.e. responds to pagp packets but does not initiate
3. Desirable: places an interface in a active negotiating state. i.e. initiates pagp packets.

##### LACP
- link aggregation control protocol
- industry standard protocol
- dynamically negotiates the creation and maintenance of ether channels
- similar to DTP for trunking.
- compatible with non-cisco switches too
- allows upto 16 interfaces to form an etherchannel but only 8 remain active and other 8 in standby mode.
###### Configuration:
```
interface range g0/0-3
channel-group 1 mode active / passive
```

**Channel Mode:**
1. ON: initiates ether channel without lacp
2. Active: places a port in an active negotiating state. In this state, the port initiates negotiations with other ports by sending LACP packets.
3. Passive: places a port in a passive negotiating state. In this state, the port responds to the LACP packets that it receives but does not initiate LACP packet negotiation.

##### Static Etherchannel
- does not use a protocol to create an ether channel but statically configures it instead

### Guidelines for configuration of ether channel:
- **EtherChannel support** - All Ethernet interfaces must support EtherChannel.
  
- **Speed and duplex** - all interfaces in an EtherChannel to operate at the same speed and in the same duplex mode.
  
- **VLAN match** - All interfaces must be assigned to the same VLAN or be configured as a trunk
  
- **Range of VLANs** - An EtherChannel supports the same allowed range of VLANs on all the interfaces in a trunking EtherChannel. If the allowed range of VLANs is not the same, the interfaces do not form an EtherChannel, even when they are set to **auto** or **desirable** mode.
  
#### LACP Config Example:
- Assigning a range of interfaces as port-channel 1 and making it a trunk interface for vlan 1, 2, and 20.

```
S1(config)# interface range FastEthernet 0/1 - 2
S1(config-if-range)# channel-group 1 mode active
S1(config-if-range)# exit
S1(config)# interface port-channel 1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk allowed vlan 1,2,20
```


## Complete config to make a port channel:

```
show interfaces status
config t
interface range fa0/1-2
switchport mode trunk
do show interfaces trunk
interface range fa0/1-2
shutdown
channel-group 1 mode {desirable/auto for PaGP, active/passive for LACP}
no shutdown
interface port-channel 1
switchport mode trunk
do show interfaces trunk
end
show etherchannel summary
```

#### Verification commands:
- `show interfaces port-channel 1`
- `show etherchannel summary`
- `show etherchannel port-channel`
- `show interfaces f0/1 etherchannel`
##### Common Issues with EtherChannel Configurations
- etherchannel interfaces not part of the same vlan
- trunking not configured on all ports
- allowed range of vlans not same 
- The dynamic negotiation options for PAgP and LACP are not compatibly configured on both ends of the EtherChannel.

