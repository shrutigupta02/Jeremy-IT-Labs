### Purpose:
- allows hosts to automatically and dynamically discover network configurations such as IP add, gateway, subnet mask, DNS server.
- typically used used for client devices like pcs
- devices like routers usually need to be manually configured
- in small networks, the router acts as the dhcp server, eg your home network

Dhcp gives IP addresses on a lease to the clients, the ip address must be given up at the end of the lease

### Four DHCP messages: DORA
client to server are BROADCAST, server to client are UNICAST or broadcast
1. **Discover Offset:** (C->S)
	- host asks if there are ay dhcp servers in the network
2. **Offer message**: (S -> C)
	- server offers an IP address to the client
3. **Request Message**: 
	   (C->S) client agrees to the offered IP and requests assignment
4. **ACK**: (S -> C), 
	   acknowledging the IP address assignment.

---
### DHCP Relay:
- basically when the dhcp server is centralised, all hosts might not be able to reach the server because broadcast messages do not leave the local subnet.
- to fix this issue, we have dhcp relay
- a router can be configured to act as a relay agent.
- it will forward the client's broadcast dhcp messages to the dhcp server

### DHCP Config

#### Server Config:
```
ip dhcp pool POOL-NAME
network <network add> /pref length
dns-server 8.8.8.8
default-router <router gateway ip>
lease days hours minutes
show ip dhcp binding
```

#### Relay Agent Config:
```
interface g0/1
ip helper-address <dhcp server ip address>
```

#### Client Config: router being a dhcp client i.e. getting a dynamic IP

```
interface g0/1
ip address dhcp
```

All commands:
![[Screenshot 2025-06-17 at 11.35.24 PM.png]]