# kurose and ross 8ed notes

**Note: this needs to be checked for accuracy as was LLM summarised**

To try out using Wireshark to capture my own traffic on wifi/ethernet

## chapter 1

### OSI model

| Layer | Layer Name | Protocols | From/To | Units |
|-------|------------|-----------|---------|-------|
| 7 | Application | HTTP, HTTPS, FTP, SMTP, DNS, DHCP, SSH, Telnet | User applications ↔ Network services | Messages, Requests |
| 6 | Presentation | SSL/TLS, JPEG, MPEG, ASCII, EBCDIC | Data formatting ↔ Application layer | Data (encrypted/compressed) |
| 5 | Session | NetBIOS, RPC, SQL, NFS, SMB | Process sessions ↔ Data streams | Sessions, Connections |
| 4 | Transport | TCP, UDP, SCTP | End-to-end communication ↔ Network routing | Segments (TCP), Datagrams (UDP) |
| 3 | Network | IP, ICMP, OSPF, BGP, RIP, ARP | Host-to-host ↔ Local delivery | Packets |
| 2 | Data Link | Ethernet, Wi-Fi (802.11), PPP, Frame Relay | Node-to-node ↔ Physical transmission | Frames |
| 1 | Physical | Ethernet cables, Fiber optic, Radio waves, Bluetooth | Electrical signals ↔ Physical medium | Bits, Electrical signals |

**Additional Context:**
- **From/To** describes the primary communication scope at each layer
- **Units** represent the data format/packaging at each layer
- Lower layers provide services to higher layers
- Each layer adds its own header (encapsulation) when sending data down the stack
- Data is de-encapsulated when moving up the stack on the receiving end

### TCP/IP Model (5 Layers)

| Layer | Name | Protocols | Units |
|-------|------|-----------|-------|
| 5 | Application | HTTP, HTTPS, FTP, SMTP, DNS, SSH | Messages |
| 4 | Transport | TCP, UDP | Segments |
| 3 | Network | IP, ICMP, ARP | Datagrams |
| 2 | Data Link | Ethernet, Wi-Fi | Frames |
| 1 | Physical | Cables, Radio waves | Bits |

**OSI vs TCP/IP:**
- OSI is a theoretical reference model
- TCP/IP is the practical implementation used on the internet
- OSI separates Presentation and Session layers, TCP/IP combines them into Application
- OSI was designed first, TCP/IP evolved from real-world networking needs

### Encapsulation in practice

Send hello via HTTP on a web browser with a GET request

#### Layer 5 - Application (HTTP)
```
GET /api/message HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0...
Accept: text/html,application/xhtml+xml...
Connection: keep-alive

Hello
```

#### Layer 4 - Transport (TCP Header)
**Binary data** that includes fields like:
- Source Port: `0x3039` (12345 in hex)  
- Dest Port: `0x0050` (80 in hex)
- Sequence Number: `0x12345678`
- Flags: `0x18` (PSH, ACK bits set)
- Window Size, Checksum, etc.

**The actual bytes might look like:**
```
30 39 00 50 12 34 56 78 87 65 43 21 50 18 FF FF A1 B2 00 00
```

#### Layer 3 - Network (IP Header)  
**More binary data:**
```
45 00 00 54 12 34 40 00 40 06 B1 E6 C0 A8 01 64 5D B8 D8 22
```
This represents version, header length, total length, identification, flags, TTL, protocol, checksum, source IP (192.168.1.100), destination IP, etc.

#### Layer 2 - Data Link (Ethernet)
**Even more binary:**
```
aa bb cc dd ee ff 11 22 33 44 55 66 08 00 [IP packet] [4-byte CRC]
```

#### Layer 1 - Physical
Electrical voltages (+5V, 0V), light pulses, or radio wave modulations - not readable text at all.

#### The Reality
- Only the Application layer might have human-readable text
- Everything below that is binary data with specific bit patterns
- Network analyzers like Wireshark decode these binary headers to show them in human-readable format
- The "Hello" text gets buried deep inside all these binary headers

Other materials covered includes delays and types of delays, calculating delay, queueing, packet loss