# Ping:
### ping indicators:
- ! -> successful
- . -> timout
- U -> destination unreachable

### Extended Ping

The **default ping** uses the **router’s nearest interface IP** as the source.  
Sometimes, you may want to specify a **different source IP**, especially for **testing alternate paths**.

To do this, Cisco IOS provides **extended ping**:
- Run `ping` in **privileged EXEC mode** without parameters
- You’ll be **prompted for inputs**, including:
    - **Source address/interface**
    - **Repeat count**, **timeout**, etc.

# Traceroute
### Verify Connectivity with Traceroute

- Each hop’s response shows how far the packet reached.
- `* * *` means a **hop didn’t respond**, which might indicate a failure or firewall filtering ICMP.

🛠 **Syntax Differences**:
- Windows: `tracert`
- Cisco IOS: `traceroute` (uses **UDP**, not ICMP like Windows)

### Options:

```
    -d                 Do not resolve addresses to hostnames.
    -h maximum_hops    Maximum number of hops to search for target.
    -j host-list       Loose source route along host-list (IPv4-only).
    -w timeout         Wait timeout milliseconds for each reply.
    -R                 Trace round-trip path (IPv6-only).
    -S srcaddr         Source address to use (IPv6-only).
    -4                 Force using IPv4.
    -6                 Force using IPv6.
```

## Network Baseline

A **network baseline** helps compare performance over time. It involves:

- Recording regular outputs of tools like **ping** and **traceroute**
    
- Saving these outputs with **timestamps** in text files for future comparison

---

# Trouble shooting:
1. identify problem
2. list causes
3. determine cause
4. plan of action
5. verify solution
6. document

# Commands:
|**Command**|**Short Description**|
|---|---|
|`ipconfig`|Displays IP config on Windows|
|`ipconfig /all`|Shows detailed IP + MAC info on Windows|
|`ipconfig /release`|Releases current DHCP IP lease|
|`ipconfig /renew`|Renews DHCP IP lease|
|`ipconfig /displaydns`|Displays cached DNS entries|
|`ifconfig` (Linux/macOS)|Shows IP & MAC of active interfaces|
|`ip address` (Linux)|Displays & manages IP addresses|
|`networksetup -listallnetworkservices`|Lists all network services (macOS)|
|`networksetup -getinfo <service>`|Shows IP settings of given service (macOS)|
|`arp -a`|Shows ARP cache (IP ↔ MAC mappings)|
|`netsh interface ip delete arpcache`|Clears ARP cache (Windows admin)|
|`show running-config`|Displays current router/switch config|
|`show interfaces`|Shows detailed interface status|
|`show ip interface`|IP-level status of interfaces|
|`show arp`|Displays ARP table on router|
|`show ip route`|Displays routing table|
|`show protocols`|Shows protocol status on interfaces|
|`show version`|Displays IOS version and device info|
|`show cdp neighbors`|Lists directly connected Cisco devices|
|`show cdp neighbors detail`|Shows neighbor IPs and more details|
|`no cdp run`|Disables CDP globally|
|`no cdp enable`|Disables CDP on specific interface|
|`show ip interface brief`|Summary of interface IPs and status|