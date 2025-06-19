each set of data flowing between a source application and a destination application is known as a conversation
## Functions:
1. track individual conversations
2. segment data
3. add header
4. identify applications using port numbers
5. convo multiplexing: multiple apps can use network layer simultaneously

### Transmission Control Protocol (TCP)

- **TCP** is a **reliable**, **connection-oriented** transport layer protocol.
  
- Stateful protocol -> tracks session throughout communication
    
- Unlike IP, which handles only packet structure, addressing, and routing, **TCP ensures all application data reaches the destination** accurately and in order.
    
- TCP adds reliability through extra fields in its header, which require more **processing at sender and receiver** ends.
    
- TCP breaks data into **segments** and manages them similarly to **tracked packages**, ensuring none are lost or out of order.
    

#### Key TCP Operations:

1. **Number and track** data segments for each host-application session.
    
2. **Acknowledge** received data.
    
3. **Retransmit** unacknowledged data after a timeout.
    
4. **Sequence** data so it can be reassembled correctly if it arrives out of order.
    
5. **Flow control** – send data at a rate acceptable to the receiver.
    

- TCP must **establish a connection** before transmitting—via a **3-way handshake**—thus it is **connection-oriented**.
  
- **TCP's buffering behavior**: If the network can’t sustain video streaming bandwidth, the player pauses and displays "buffering..." until enough data is received.

### User Datagram Protocol (UDP)

- **UDP** is a **simpler, connectionless**, and **stateless** transport layer protocol.
    
- It has **low overhead** due to fewer header fields and **no reliability features**.
    
- UDP also breaks data into **datagrams** (also referred to as segments).
    

#### Characteristics of UDP:

- **No acknowledgements**.
    
- **No retransmissions**.
    
- **No sequence tracking**.
    
- **Faster** and more efficient due to the lack of reliability checks.
    
- Compared to TCP, UDP is like mailing a **regular untracked letter**—you send it without knowing if the receiver is available or if it will arrive.

## TCP Features:
1. Establish session
2. Reliable delivery
3. Ensure in-order data
4. Supports flow control

## TCP Header: 20 bytes
1. Source and destination ports
2. sequence number
3. acknowledgement number
4. header length
5. control flags
6. window size
7. checksum
8. urgent

### UDP:
#### Key Characteristics of UDP:

- **No session establishment**: Data is sent immediately, without negotiating a connection.
    
- **No retransmissions**: If segments are lost during transmission, they are not resent.
    
- **No flow control**: The sender is **not aware** of the receiver's resource availability.
    
- **Order is not guaranteed**: Data is processed in the **order received**, even if that order is incorrect.
    
- **No acknowledgments or tracking**: UDP does not maintain state or confirm successful delivery.

### UDP Header: 8 bytes
1. Source
2. Destination
3. Header length
4. checksum
   
what apps use udp:
- voip, VC
- DNS
- DHCP
- SNMP
- TFTP

## Multiple separate communications:
both tcp and udp can maintain multiple communications to a network.

### Socket pairs:
socket  = ip address + port number
socket pair = source socket + destination socket

#### Purpose of Sockets:
- Enable a host to:
    - **Distinguish multiple processes/connections**
    - Route **responses** correctly to the initiating application using the **source port** as a return address

## Port numbers: 16 bit
assigned by IANA

three groups:

| Name            | Port         |
| --------------- | ------------ |
| Well Known      | 0-1023       |
| Registered      | 1024-49,151  |
| Dynamic/Private | 49,152-65535 |
some OS use registered ports as private ports

## TCP Connection Management

### 3 way handshake
SYN, SYN-ACK, ACK

### 4-way termination
FIN, ACK, FIN, ACK

#### Purpose of the Three-Way Handshake:

1. Confirms the **destination is reachable** on the network.
2. Verifies the **destination has an active service** on the intended **port number**.
3. Notifies the destination of the client’s **intent to establish a session**.

## TCP Flags:
|**Flag**|**Meaning**|
|---|---|
|**URG**|Urgent pointer field is significant (for urgent data)|
|**ACK**|Acknowledgment field is significant (used in handshake and normal data)|
|**PSH**|Push function – deliver data to application immediately|
|**RST**|Reset the connection (error or timeout)|
|**SYN**|Synchronize sequence numbers (used to initiate a session)|
|**FIN**|No more data from sender (used to terminate a session)|
## TCP Flow Control:
#### Sequence Numbers:

- Every TCP segment includes a **sequence number** in its header, which represents the **first byte of data** in the segment.
    
- During the **three-way handshake**, an **Initial Sequence Number (ISN)** is chosen—usually a **random number** (for security).
    
- For simplicity, examples may start with ISN = 1.

#### Reassembly Process:

- Segments are stored in a **receiving buffer** upon arrival.
    
- If received **out of order**, segments are held until **missing segments** arrive.
    
- Once all data is available, it’s **reassembled** in the correct order and passed to the application.

#### Window Size:

- A **16-bit field** in the TCP header.
    
- Determines how much data (in bytes) the sender can transmit **before needing an acknowledgment**.
    
#### Example:

- PC B’s **initial window size**: 10,000 bytes.
- PC A can send bytes 1 to 10,000.    
- After processing, PC B sends an **ACK for byte 2,921**.
- PC A’s send window **slides** forward to byte 12,920 (i.e., adds 2,920 bytes).
    
###### This technique is called **sliding windows**—as ACKs are received, the sender **continues sending more data**, keeping the pipeline full.

##### Buffer Awareness:
- If the destination’s buffer fills up, it can **reduce its window size** to tell the sender to slow down.

### MSS: max segment size
how much data can be accepted in one segment excluding headers

usually: 1460

