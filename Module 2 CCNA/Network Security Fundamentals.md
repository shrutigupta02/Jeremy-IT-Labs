# Threats:
## Types:
1. Info theft
2. Data loss
3. Identity theft
4. Disruption of service

# Vulnerabilities:
## Types:
##### 1. Technical:
1. TCP/IP -> HTTP, FTP, ICMP are all insecure
2. OS
3. Network equipment

##### 2. Configurational: related w passwords and auth
##### 3. Policy: 
- no disaster recovery
- poor employee training
- no monitoring 
- bad auth

# Physical Security:
- **Hardware threats** - This includes physical damage to servers, routers, switches, cabling plant, and workstations.
- **Environmental threats** - This includes temperature extremes (too hot or too cold) or humidity extremes (too wet or too dry).
- **Electrical threats** - This includes voltage spikes, insufficient supply voltage (brownouts), unconditioned power (noise), and total power loss.
- **Maintenance threats** - This includes poor handling of key electrical components (electrostatic discharge), lack of critical spare parts, poor cabling, and poor labeling.

# Malwares:
1. virus
2. worm
3. trojan horse

## Reconnaissance Attacks: network attack
**The discovery and mapping of systems, services, or vulnerabilities.**

Threat actors use nslookup or whois commands to find the gateway addresses and then use the ping tool to ping all possible ip addresses of a subnet, trying to find the active ones

TOOLS:
- nslookup / whois
- ping sweep
- port scan

## Access Attacks:
gaining unauthorised access
1. password access attacks
2. trust exploitation
3. port redirection
4. man in the middle

# Security Measures:
1. keep backups
2. update regularly
3. AAA - Authenticate, Authorise and Account
4. Firewalls
5. End point security -> antiviruses in end hosts etc

### Types of firewalls:
1. packet filtering
2. application filtering
3. url filtering
4. stateful packet inspection - only allow packets that are responses to requests from internal network

# Cisco Autosecure
cisco routers use this to strengthen the security 

`R1(config)# auto secure`

**Additional Manual Security Practices:**

- Change default usernames/passwords immediately.
- Restrict system resource access to authorized users.
- Uninstall/disable unused services and applications.
- Ensure software/firmware is **patched** and **up-to-date**.

**Cisco Password Behavior:**
- **Leading spaces** are ignored, but **mid-phrase spaces** are accepted and strengthen the password.

### **Additional Password Security**

- **Encrypt all plaintext passwords**:
  `R1(config)# service password-encryption`
    
- **Set minimum password length**:
    `R1(config)# security passwords min-length 8`
    
- **Prevent brute-force login attacks**:
    `R1(config)# login block-for 120 attempts 3 within 60`
    - This blocks login for 120 seconds after 3 failed attempts within 60 seconds.
        
- **Set inactivity timeout for privileged EXEC mode**:
    `R1(config)# line vty 0 4 R1(config-line)# exec-timeout 5 30`

# SSH:

```
hostname R1
ip domain name span.com
crypto key generate rsa general-keys modulus 1024
username Bob secret cisco
line vty 0 4
login local
transport input ssh
exit
```

### Extra: 
disable unused interfaces:
``` 
show ip ports all
```

disable http

```
no ip http server
```

