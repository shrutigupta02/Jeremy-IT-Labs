#### Purpose:

- used to resolve human readable names from IP addresses.

### DNS Cache:
- devices will save DNS server's responses in local DNS Cache.
- so you wont have to query dns server everytime you access a url

## Configure Router as DNS server:

```
ip dns server

//records:
ip host R1 <ip address>
ip host PC1 <ip add>

ip name-server 8.8.8.8 
// this command lets router query google's dns server if it finds a name not present in its records

ip domain lookup
```

show all entries in dns:
`show hosts`

![[Screenshot 2025-06-17 at 10.39.13 PM.png]]