### IP Classes:

| Class | First Octet | Prefix | Networks  | Hosts |
| ----- | ----------- | ------ | --------- | ----- |
| A     | 0           | /8     | 2^7 = 128 | 2^24  |
| B     | 128         | /16    | 2^14      | 2^16  |
| C     | 192         | /24    | 2^21      | 2^8   |
| D     | 224         | -      | -         | -     |
| E     | 240         | -      | -         | -     |
##### Issue with classful addressing:
- Wastes IP addresses
- IPv4 addresses are being exhausted. 

#### Private IP Addresses:
1. 10.0.0.0 - 10.255.255.255
2. 172.16.0.0 - 172.31.255.255
3. 192.168.0.0 - 192.168.255.255 
### CIDR - Classless Inter Domain Routing:
Class based prefix lengths are not required, allowing larger networks to be split into smaller networks. These smaller networks are called subnetworks.

>[!note]
>2^n - 2 = usable address
>n = host bits

_/31 and /32 are special because :_

- _/31_ has only 2 addresses and after removing network and broadcast there are 0 usable addresses, however is the network is assigned to Point to Point network i.e there are only two devices connected then it can indeed be used here as there is no need for network and broadcast address
- _/32_ has zero usable addresses as it is only one address, it is used rarely however use may use it to create a static route to a single host instead of a whole network

**Subnetting Trick**:
- for a /26 network:
	- last octet looks like:
		- 00 (network bits) | 000000 (host bits)
	- hence if the network bits have 4 possible combo: 00, 01, 10, 11 then you make make 4 subnets.
- similarly for a /25 network you can make 2 subnets. 

**Trick 2:**
- if network bits are: 00 hence binary for 64 (01000000 = 64), hence if you make equal sized subnets then they'd be:
	- x.y.z.0
	- x.y.z.64
	- x.y.z.128 (keep adding 64)
	- x.y.z.192
- similarly if networks bits are 000 -> 32 then:
	- x.y.z.0
	- x.y.z.32
	- x.y.z.64 (keep adding 32)
	  
**Trick 3:**
- what subnet does x.y.z.a/b belong to? 
	- write down the address in binary and set all host bits to 0. you get your answer.

![[Screenshot 2025-06-12 at 10.54.46 PM.png]]

>[!important]
>watch subnetting part 2 video before OA

All of the above were FLSM - Fixed Length Subnet Mask

---
## VLSI (Variable Length Subnet Masks)

This means creating subnet of various sizes to make use of networks more efficient **Steps :**

- assign the largest subnet at the start of the address space
- then second largest
- repeat until all subnets have been assigned