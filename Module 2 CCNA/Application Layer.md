AL: provides the interface between the applications used to communicate, and the underlying network over which messages are transmitted
Presentation L: formatting, compressing, encrypting
Session L: handles the exchange of information to initiate dialogs, keep them active, and to restart sessions that are disrupted or idle for a long period of time

### **Client-Server Model**

- In a client/server model, the **client** requests information and the **server** responds.
- Both client and server are considered part of the **application layer**.
- The client initiates the communication, and the server sends one or more data streams back.
- Application layer protocols define how requests and responses are structured.
- Data going **from client to server** is called an **upload**, and **from server to client** is a **download**
- Files are **downloaded** from server to client in this model.

### **Peer-to-Peer Networks**

- In a **P2P network**, devices (peers) access data directly from each other **without a dedicated server**.
    
- There are two concepts: **P2P networks** and **P2P applications** — they are similar in some aspects but function differently.
    
- In P2P networks:
    
    - **Two or more computers** are connected and share resources (like files or printers).
    - Each device (peer) can act as both **client and server**, depending on the request.
    - One computer can be a server for one task while being a client for another.

- The **client/server roles are dynamic**, set **per request**.
    
- P2P networks also support things like **networked games** or **internet connection sharing**.
    
- All devices are **equal in communication**.

### Peer-to-Peer Applications

- P2P applications let a device act as both **client and server in the same communication**.
    
- Every client is a server, and every server is a client.
    
- These apps need to:
    
    - Have a **user interface**.
        
    - Run a **background service**.
        
- Some P2P apps use a **hybrid system**:
    
    - Resource sharing is decentralized.
        
    - A **centralized directory** holds **indexes** (pointers to where resources are).
        
    - Peers consult this directory to find where resources are stored on other peers.

### **Common P2P Applications**

- Each device running a P2P application can act as a **client or server** to others.
    
- **Examples of common P2P networks**:
    
    - **BitTorrent**
        
    - **Direct Connect**
        
    - **eDonkey**
        
    - **Freenet**
        
- Some use the **Gnutella protocol**:
    
    - Users **share entire files** with each other.
        
    - Gnutella-compatible software connects users over the internet.
        
    - Resources can be found and downloaded from peers.
        
    - Popular Gnutella clients include:
        
        - **μTorrent**
            
        - **BitComet**
            
        - **DC++**
            
        - **Deluge**
            
        - **eMule**
            
- Many P2P apps **share parts of many files simultaneously**:
    
    - Users rely on a **torrent file** to find peers with needed file pieces.
        
    - The torrent file also contains data about **tracker servers** that track who has which file parts.
        
    - Clients request pieces from **multiple users at once** (called a **swarm**).
        
    - This system is known as **BitTorrent**.
        
    - BitTorrent has its own client, but others like **uTorrent**, **Deluge**, and **qBittorrent** also exist.

## DNS Message Format:
- **A** - An end device IPv4 address
- **NS** - An authoritative name server
- **AAAA** - An end device IPv6 address (pronounced quad-A)
- **MX** - A mail exchange record

#### Message Section:
1. Question: sending a query to DNS server
2. Answer: resource records answering question
3. Authority: resource records pointing to authority
4. Additional: additional info

## FTP and SMB:
 #### SMB: using this the client can access the resources on the server as though the resource is local to the client host.
 - Start, authenticate, and terminate sessions.
- Control file and printer access.
- Allow an application to send or receive messages to or from another device.

##### FTP: 
requires 2 connections:
1. control connection: c -> s
2. data connection: c <-> s

