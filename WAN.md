### 🌐 **WAN Basics**

- **LAN**: Local, small-area network.
- **WAN**: Connects LANs over large geographical areas.
- **Private WAN**: Dedicated, secure, consistent bandwidth.
- **Public WAN**: Over Internet, variable performance, less secure.

---

### 🧭 **WAN Topologies**

- **Point-to-Point**: Direct connection; costly for many sites.
- **Hub-and-Spoke**: Central hub connects to branches.
- **Dual-homed**: 2 connections to improve redundancy.
- **Mesh (Full/Partial)**: All nodes interconnected (fully/partially).

---

### 🏢 **Carrier Connections**

- **Single-Carrier**: One provider; single point of failure.
- **Dual-Carrier**: Two providers; better reliability.

---

### 🔄 **Evolving Networks**

- Networks grow from small LAN → campus → branch → distributed.
- Use planning, scalable tech, optimized designs.

---

### ⚙️ **WAN Standards**

- Defined by **TIA/EIA, ISO, IEEE**.
    
- Focus on **Layer 1 (physical)** and **Layer 2 (data link)**.
    

---

### 🔌 **Layer 1 Protocols**

- **SDH, SONET**: Long-distance fiber optic transmission.
    
- **DWDM**: Multiplies data capacity over one fiber strand.
    

---

### 📦 **Layer 2 Protocols**

- **Modern**: DSL, Cable, Ethernet WAN, MPLS.
    
- **Legacy**: PPP, HDLC, Frame Relay, ATM.
    

---

### 🧱 **WAN Devices**

- **CPE**: Customer equipment.
    
- **DCE/DTE**: Communication endpoints.
    
- **Modems, CSU/DSU, DSLAM**: Convert and route signals.
    

---

### ⚡ **Transmission Types**

- **Serial**: One bit at a time (used in WANs).
    
- **Parallel**: Multiple bits (short distance only).
    

---

### ☎️ **Circuit-Switched WANs**

- **PSTN**: Analog dial-up, <56 kbps.
    
- **ISDN**: Digital circuit-switched; 45 Kbps – 2 Mbps.
    

---

### 📡 **Packet-Switched WANs**

- **Frame Relay**: PVC-based, 4 Mbps.
    
- **ATM**: Fixed 53-byte cells; used for voice/video.
    
- **Replaced** by faster Ethernet and MPLS.
    

---

### 🚀 **Modern WANs**

- **Dedicated**: **Dark Fiber** (very fast, very expensive).
    
- **Packet-switched**: **MPLS** – labels for fast routing, QoS, VPN.
    
- **Ethernet WAN**: Fiber-based, scalable, integrates well with LANs.

---

### 🌍 **Internet-Based Connectivity**

- **Wired**: DSL, Cable, Optical Fiber.
    
- **Wireless**: Cellular (3G–5G), Satellite, Municipal Wi-Fi.
    

---

### 🔁 **DSL**

- Uses phone lines.
    
- **ADSL**: Higher download speed.
    
- **SDSL**: Equal speeds.
    
- Connects to **DSLAM** at provider’s end.
    

---

### 🧾 **DSL + PPPoE**

- **PPPoE**: Used for authentication, IP assignment, QoS.
    
- Can run on host or router.
    

---

### 📺 **Cable**

- Uses **DOCSIS** over hybrid fiber-coaxial (HFC).
    
- Shared bandwidth → slower during peak usage.
    

---

### 💡 **FTTx (Fiber to x)**

- **FTTH**: Direct to home.
    
- **FTTB/FTTN**: Fiber to building or nearby node.
    
- Highest bandwidth option.
    

---

### 📶 **Wireless Internet**

- **Municipal Wi-Fi**: Public or private use.
    
- **Cellular**: 3G/4G/5G.
    
- **Satellite**: For remote areas; slower and expensive.
    
- **WiMAX**: Alternative fixed/mobile broadband.
    

---

### 🔐 **VPNs**

- Secure, encrypted tunnel over public internet.
    
- **Site-to-site**: Configured in routers.
    
- **Remote access**: User-initiated via software.
    

---

### 🛜 **ISP Connectivity Options**

- **Single-homed**: One ISP, no redundancy.
    
- **Dual-homed**: Two links, same ISP.
    
- **Multihomed**: Two ISPs.
    
- **Dual-multihomed**: Redundant links with multiple ISPs.
    

---

### ✅ **Broadband Comparison**

|Type|Pros|Cons|
|---|---|---|
|DSL|Not shared, stable|Distance-sensitive, slower upload|
|Cable|Widely available|Shared bandwidth|
|Fiber|Fastest, stable|Expensive installation|
|Cellular|Wireless, mobile|Limited coverage|
|Wi-Fi|Inexpensive or free|Limited range|
|Satellite|Available everywhere|High latency, expensive|

---
### 🔒 **Remote-Access VPN**

- Connects remote users securely to enterprise.
    
- Two types:
    
    - **Clientless VPN** – uses browser (SSL/TLS).
        
    - **Client-based VPN** – uses VPN software (IPsec or SSL).
        

---

### 🧩 **SSL vs IPsec VPN**

- **SSL (TLS)**: Easier to deploy, ideal for web apps.
    
- **IPsec**: More secure, supports full network access.
    
- Organizations may use both.
    

---

### 🏠 **Site-to-Site VPN**

- Connects entire networks via VPN gateways.
    
- Uses **IPsec** to encrypt & encapsulate traffic.
    
- End users are unaware of the VPN.

---

### 📦 **GRE over IPsec**

- GRE encapsulates multicast traffic (e.g., OSPF).
    
- GRE packet is encrypted using IPsec.
    
- Solves IPsec’s unicast-only limitation.

---

### 🔁 **DMVPN (Dynamic Multipoint VPN)**

- Scalable, uses **mGRE + IPsec**.
    
- Central **hub** connects to **spokes**.
    
- Spokes can form **direct tunnels** with each other.


---

### 📡 **IPsec Virtual Tunnel Interface (VTI)**

- VPN configuration bound to **virtual interface**.
- Supports **unicast + multicast**.
- No GRE needed for routing protocols.

---

### 🏢 **MPLS VPN (Layer 2 & 3)**

- Provided by ISPs using label switching.
    
- **L3 MPLS VPN**: ISP participates in routing.
    
- **L2 MPLS VPN**: ISP emulates LAN; customer manages routing.
    
- Secure like traditional WANs.
    

---

