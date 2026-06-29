# 🌐 Computer Networks — Complete Placement Notes

> 📌 **How to use this file:** Read top-to-bottom for concepts → use the 🧠 *Interview Bytes* boxes for quick revision → run the 💻 code blocks (C++ & Java side-by-side) to *see* the algorithms work, not just memorize them.

---

## 📑 Table of Contents
1. [Introduction](#1-introduction)
2. [Networking Devices](#2-networking-devices)
3. [Physical Layer](#3-physical-layer)
4. [Data Link Layer](#4-data-link-layer)
5. [Network Layer](#5-network-layer)
6. [Transport Layer](#6-transport-layer)
7. [Session / Presentation / Application Layer](#7-session--presentation--application-layer)
8. [Delays](#8-delays)
9. [💻 Code Lab (C++ & Java)](#9-code-lab-c--java)
10. [🎯 Placement Interview Question Bank](#10-placement-interview-question-bank)
11. [✅ MCQ Sprint + Answer Key](#11-mcq-sprint--answer-key)

---

## 1. Introduction

### 🔹 What is a Computer Network?
A **group of interconnected nodes (devices)** that share resources & information via communication links.

```
┌────────┐ link ┌────────┐ link ┌────────┐
│  PC-A  │◀────▶│ Router │◀────▶│  PC-B  │
└────────┘      └────────┘      └────────┘
   node          intermediary       node
```

| Term | Meaning | Examples |
|---|---|---|
| **Node** | device that sends/receives data | End: PC, printer, smartphone · Intermediary: Router, Bridge, Modem |
| **Link** | medium to transfer data | Wired (coaxial), Wireless (Wi-Fi, Bluetooth) |
| **Protocol** | common rule-set for sender & receiver | TCP, HTTP, FTP... |

### 🔹 Characteristics
⚡ Fault Tolerance · 📈 Scalability · 🎯 QoS · 🔒 Security

### 🔹 Elements of a Protocol
`Message Encoding → Formatting/Encapsulation → Timing → Size → Delivery Options`

### 🔹 Data Flow Types

```
Simplex     A ───▶ B                (keyboard → CPU)
Half Duplex A ◀──▶ B (one at a time) (walkie-talkie)
Full Duplex A ⇄ B  (simultaneous)    (telephone)
```

### 🔹 Types of Networks

| | PAN | LAN | CAN | MAN | WAN |
|---|---|---|---|---|---|
| Full form | Personal | Local | Campus | Metropolitan | Wide |
| Range | 1–100 m | up to 2 km | 1–5 km | 5–50 km | >50 km (global) |
| Tech | Bluetooth, Zigbee | Ethernet, Wi-Fi | Ethernet, Fiber | Metro Ethernet, MPLS | Leased line, VPN, Satellite |
| Speed | Very High | Very High | High | Average | Low |
| Cost/Error | Very Low | Low | Moderate | High | Very High |

### 🔹 Client-Server vs Peer-to-Peer

```
CLIENT-SERVER                 PEER-TO-PEER
   Client ──Req──▶ Server         PC1 ⇄ PC2
   Client ◀──Res── Server          ⇕     ⇕
                                  PC3 ⇄ PC4
(centralized, secure,         (decentralized, each node
 easily scalable)              is client + server)
```

🧠 **Interview Byte:** P2P is *not* infinitely scalable — too many peers = instability (used in BitTorrent, Blockchain).

### 🔹 Topologies — Visual Cheat Sheet

```
BUS:    A───B───C───D───E          (single shared cable, cheap, single point of failure)

RING:   A───B
        │    │
        D───C                     (token passes in loop, one break = network down)

STAR:   A   B   C
         \  │  /
          [HUB]                   (central point, easy fault isolation)
         /  │  \
        D   E   F

MESH:   A───B
        │ ╲ │ ╲
        C───D───E                 (every node ↔ every node, most reliable, most $$)

TREE:        ROOT
             /  \
           SW1   SW2
           / \    / \
          A   B  C   D            (hierarchical, university campuses)

HYBRID: combination of any of the above (Bus + Ring + Star)
```

| Topology | 👍 Pros | 👎 Cons | Use Case |
|---|---|---|---|
| Bus | Cheap, simple | One break kills network | Small/temp setups |
| Ring | Fast, simple | One failure = total outage | Token networks |
| Star | Fault isolation, easy setup | Hub failure = outage | Homes/Offices |
| Mesh | Most reliable, redundant | Expensive, complex wiring | Critical systems |
| Tree | Scalable, hierarchical | Backbone failure = segment down | Campuses |
| Hybrid | Flexible, customizable | Complex, costly | Large tailored nets |

### 🔹 Switching Techniques

```
CIRCUIT SWITCHING   : [Setup]→[Dedicated path reserved]→[Data]→[Teardown]
                        (Telephone call — reliable but wastes idle resources)

MESSAGE SWITCHING    : Whole message stored & forwarded hop-by-hop
                        (Email — not for real-time)

PACKET SWITCHING     : Data → small packets → routed independently
                        (Internet — efficient, scalable)
```

| Feature | Datagram Approach | Virtual Circuit Approach |
|---|---|---|
| Connection | Connectionless | Connection-oriented |
| Routing | Each packet, different route | All packets, same route |
| Addressing | Full destination address | Virtual circuit ID (short) |
| Reliability | Lower (out of order/loss) | Higher (in order) |
| Example | **IP** | ATM, Frame Relay |

---

## 2. Networking Devices

```
   Hardware only      Hardware + Software        Software
   ────────────       ──────────────────         ────────
   Cable, Repeater,    Bridge, Switch,            Gateway, IDS,
   Hub, Modem          Router                     Firewall
```

### 🔹 Device Cheat Sheet (OSI Layer Mapping)

| Device | OSI Layer | Job | Key Fact |
|---|---|---|---|
| **Repeater** | 1 (Physical) | Boosts weak signal | No memory, just amplifies (+ noise) |
| **Hub** | 1 (Physical) | Multiport repeater | Broadcasts to all ports → high collisions |
| **Bridge** | 2 (Data Link) | Connects 2 LAN segments | Filters by MAC table (store & forward) |
| **Switch** | 2 (Data Link) | Smart multiport bridge | 1 collision domain *per port* (full-duplex) |
| **Router** | 3 (Network) | Connects networks | Routes by IP, uses routing table |
| **Gateway** | Mostly 7 (App) | Connects different protocol networks | e.g., VoIP ↔ Telephone network |
| **Firewall** | Varies | Filters traffic | Rule-based (IP/Port/Protocol) |
| **IDS** | Varies | Detects, does NOT block | NIDS (network) vs HIDS (host) |

```
Repeater vs Amplifier
──────────────────────
Repeater  : restores signal to ORIGINAL strength + removes noise
Amplifier : just boosts power (boosts noise too!)
```

### 🔹 Bridge Example (MAC Filtering Logic)

```
        P1 (M1,M2,M3,M4) ── [BRIDGE] ── P2 (M5,M6,M7,M8)

M1 → M3  : same side(P1)  → Bridge does NOT forward to P2  ✅ traffic saved
M1 → M7  : other side(P2) → Bridge forwards to P2          ✅ delivered
```

| Feature | Static Bridge | Dynamic (Transparent) Bridge |
|---|---|---|
| Setup | Manual | Self-learning |
| MAC Table | Manual | Auto-built |
| Flexibility | Low | High |

🧠 **Interview Byte:** A **Switch** creates *separate collision domains per port* — that's why switches don't suffer collisions like hubs do.

---

## 3. Physical Layer

📍 **Layer 1** (bottom) · 📦 **Data unit = Bits**

```
   Devices here: Cables | Hub | Repeater
```

### 🔹 Services
```
1. Bit-by-bit Transmission
2. Encoding/Decoding     (data ⇄ signal)
3. Signal Transmission   (analog/digital)
4. Modulation/Demodulation  (Modem does this)
```

### 🔹 Connection Types
```
Point-to-Point:  A ────── B
Multipoint:      A ─┬── B
                     ├── C
                     └── D
```

### 🔹 Communication Modes

```
UNICAST     1 ──▶ 1        (you streaming Netflix)
ANYCAST     1 ──▶ nearest  (CDN server selection)
MULTICAST   1 ──▶ group    (video conference)
BROADCAST   1 ──▶ all      (DHCP, ARP)
```

---

## 4. Data Link Layer

📍 **Layer 2** · 📦 **Data unit = Frames**

```
   Data Link Layer
   ┌─────────────────────┐
   │   LLC sublayer       │  → flow control, error notify, ACKs
   ├─────────────────────┤
   │   MAC sublayer        │  → addressing, medium access
   └─────────────────────┘
```

### 🔹 Frame Structure

```
┌──────┬─────────┬──────────────┬─────────┬──────┐
│ Flag │ Header  │ Payload      │ Trailer │ Flag │
└──────┴─────────┴──────────────┴─────────┴──────┘
        (src/dst MAC) (actual data) (CRC/error bits)
```

### 🔹 Framing Types
```
Fixed-Size   → no delimiter needed (length defines boundary)
Variable-Size → needs delimiter
   ├─ Length Field
   └─ End Delimiter ── Byte Stuffing (char) / Bit Stuffing (bit)
```

### 🔹 Flow Control Protocols

```
STOP-AND-WAIT (Window=1)
Sender: [Frame]──▶          ◀──[ACK]   ... repeat (idle while waiting ❌ slow)

GO-BACK-N (Sliding Window, size N)
Send 0,1,2,3 → ACK(0) → slide → send 4 ...
If frame 2 lost → resend 2,3,4,5... (even correct ones - wasteful)

SELECTIVE REPEAT
Send 0,1,2,3 → if 2 lost → NACK(2) → resend ONLY frame 2 (efficient ✅)
```

| Protocol | Retransmits | Efficiency |
|---|---|---|
| Stop-and-Wait | 1 frame at a time | Low |
| Go-Back-N | From lost frame onward (even correct ones) | Medium |
| Selective Repeat | Only the lost/damaged frame | High |

### 🔹 Error Detection & Correction

```
ERROR DETECTION                     ERROR CORRECTION
├─ Parity Check (1-bit)             ├─ Backward (ask for retransmit)
├─ 2D Parity                        └─ Forward (Hamming Code — self-fix)
├─ Checksum
└─ CRC (most robust, polynomial)
```

🧠 **Interview Byte:** Parity check catches **odd** number of bit-flips. It MISSES 2-bit (even) errors!

### 🔹 Multiple Access Control (MAC) — The Big Picture

```
                    Multiple Access Protocols
        ┌─────────────────┬──────────────────┬──────────────────┐
   Random Access      Controlled Access   Channelization
   ALOHA, CSMA,        Reservation,        FDMA, TDMA, CDMA
   CSMA/CD, CSMA/CA    Polling, Token Pass
```

```
PURE ALOHA      : send anytime → collision? wait random Tb → retry
                  Vulnerable Time = 2·tp

SLOTTED ALOHA   : send only at slot start → fewer collisions
                  Vulnerable Time = tp

CSMA            : "listen before talk" (still has propagation-delay collisions)
CSMA/CD         : detect collision mid-transmit → JAM signal → backoff
                  (classic Ethernet, NOT used in modern switched LANs)
CSMA/CA         : avoid collision via IFS + random backoff + ACK
                  (used in Wi-Fi, since wireless can't reliably "detect")
```

| Mode | Behavior |
|---|---|
| 1-Persistent | idle→ transmit immediately (100%) |
| p-Persistent | idle→ transmit with probability p |
| 0-Persistent | priority/order based wait |

```
Controlled Access:
  Reservation  → mini-slots to "book" your turn
  Polling      → central controller asks each station in turn
  Token Passing→ token circles a logical ring; only holder transmits
```

```
Channelization:
  FDMA → split by FREQUENCY  (radio)
  TDMA → split by TIME       (GSM/2G)
  CDMA → split by CODE       (3G, shared freq+time, unique codes)
```

### 🔹 Ethernet Frame Format

```
┌──────────┬─────┬─────────────┬────────┬────────┬───────────┬─────┐
│ Preamble │ SFD  │ Dest Addr   │ Src    │ Length │ Data      │ CRC │
│ 7 Bytes  │1Byte │  6 Bytes    │6 Bytes │2 Bytes │46-1500 B  │4 B  │
└──────────┴─────┴─────────────┴────────┴────────┴───────────┴─────┘
```

🧠 **Interview Byte:** Min Ethernet frame = 64 bytes (14 header + 46 data + 4 CRC) — padding added if data < 46B, so collisions can be detected in time (CSMA/CD requirement).

---

## 5. Network Layer

📍 **Layer 3** · 📦 **Data unit = Packets**

### 🔹 Services
`Routing · Logical(IP) Addressing · Packet Switching · Forwarding · Fragmentation/Reassembly · Subnetting · NAT`

### 🔹 IP Address Classes (Classful Addressing)

```
Class A:  0xxxxxxx  →  0 – 127     | Network.Host.Host.Host  | 255.0.0.0
Class B:  10xxxxxx  → 128 – 191    | Network.Network.Host.Host | 255.255.0.0
Class C:  110xxxxx  → 192 – 223    | Net.Net.Net.Host        | 255.255.255.0
Class D:  1110xxxx  → 224 – 239    | Multicast
Class E:  1111xxxx  → 240 – 255    | Reserved/Research
```

| Class | Networks | Hosts/Network | Use |
|---|---|---|---|
| A | 128 | 16,777,214 | Huge orgs |
| B | 16,384 | 65,534 | Medium/Large orgs |
| C | 2,097,152 | 254 | Small orgs |

```
Network ID  = all host bits 0      → e.g. 192.168.1.0
Broadcast   = all host bits 1      → e.g. 192.168.1.255
Usable Hosts= everything in between
```

### 🔹 CIDR (Classless Addressing)

```
192.168.1.0/28  →  28 bits network, 4 bits host
Usable Hosts = 2^4 - 2 = 14
```

📌 CIDR Block Rules: ① contiguous IPs ② size = power of 2 ③ first IP divisible by block size

### 🔹 Subnetting

```
Subnets = 2^(borrowed bits)
Hosts   = 2^(remaining host bits) - 2
```

| | FLSM | VLSM |
|---|---|---|
| Mask | Same for all subnets | Different per subnet |
| Efficiency | Less (wastes IPs) | More (matches need) |
| Used with | Classful | Classless (CIDR) |

### 🔹 IPv4 vs IPv6 Header

| Feature | IPv4 | IPv6 |
|---|---|---|
| Size | Variable 20–60 B | Fixed 40 B |
| Address | 32-bit | 128-bit |
| Checksum | Present | ❌ Removed (speed) |
| Fragmentation | Router + Host | Source only |
| Extensions | Options field | Extension Headers |
| TTL name | TTL | Hop Limit |

### 🔹 Routing

```
Routing Protocols (unicast)
        │
  ┌─────┴──────┐
Intra-domain   Inter-domain
  │                │
┌─┴───┐         Path Vector (BGP)
Distance  Link
Vector    State
(RIP)     (OSPF)
```

| | RIP (Distance Vector) | OSPF (Link State) | BGP (Path Vector) |
|---|---|---|---|
| Metric | Hop count | Cost (bandwidth) | Path attributes |
| Scope | Small networks | Large networks | Between ASes (Internet) |
| Issue | Count-to-Infinity (slow convergence) | Needs CPU/memory | Complex, slow convergence |

```
COUNT-TO-INFINITY PROBLEM (RIP)
Link down → hop count climbs 14→15→16...∞  → routing loop
```

### 🔹 ARP (Address Resolution Protocol)
```
"I know your IP, but what's your MAC?"
1. Check ARP cache
2. If miss → broadcast ARP Request "Who has 192.168.0.12?"
3. Target replies with MAC
4. Sender caches it, sends data via MAC
```

### 🔹 NAT (Network Address Translation)
```
Private IP (10.0.0.1) ──[Router+NAT]── Public IP (150.150.0.1) ── Internet
        Translates Private ⇄ Public using a NAT table
```

---

## 6. Transport Layer

📍 **Layer 4** · 📦 **Data unit = Segments**

```
Socket Address = IP Address + Port Number
192.168.1.10 : 80   → Web server (HTTP)
192.168.1.10 : 21   → FTP server
(same IP, different port = different app!)
```

### 🔹 TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Reliability | Reliable (ACK+retransmit) | Unreliable |
| Speed | Slower | Faster |
| Header | 20–60 B | 8 B (fixed) |
| Use case | Web, Email, File Transfer | Video calls, Games, DNS, Streaming |
| Example | HTTP, FTP, SMTP | DNS, Zoom, WhatsApp calls |

### 🔹 TCP 3-Way Handshake

```
CLIENT                          SERVER
  │── SYN (seq=9000) ──────────▶│
  │◀── SYN+ACK (seq=3000,ack=9001)│
  │── ACK (ack=3001) ──────────▶│
        ✅ Connection Established
```

### 🔹 TCP Connection Termination (4-way, simplified to graceful 3/4 step)

```
CLIENT                          SERVER
  │── FIN ─────────────────────▶│
  │◀── ACK ─────────────────────│   (half-close)
  │◀── FIN ─────────────────────│
  │── ACK ─────────────────────▶│
        ✅ Connection Closed
```

🧠 **Interview Byte:** `SYN`=start · `ACK`=confirm · `FIN`=close · `RST`=force-kill.

### 🔹 TCP Header Quick View

```
┌──────────┬──────────┬───────────────┬──────────────┐
│ SrcPort  │ DestPort │ Sequence #    │ Ack #        │
├──────────┴──────────┴───────────────┴──────────────┤
│ HLEN│Flags(URG ACK PSH RST SYN FIN)│ Window Size   │
├──────────────────────────────────────────────────────┤
│ Checksum │ Urgent Pointer │ Options(MSS, etc.)       │
└──────────────────────────────────────────────────────┘
```

```
Segments Needed = Window Size / MSS
e.g. 18000 / 1800 = 10 segments
```

---

## 7. Session / Presentation / Application Layer

### 🔹 Session Layer (L5)
`Session Establishment → Synchronization (checkpoints) → Dialog Control (half/full duplex) → Resync (Set/Abandon/Restart)`

### 🔹 Presentation Layer (L6) — "Translator"
```
Data Translation  (ASCII ⇄ UTF-8)
Compression       (Lossy = permanent loss | Lossless = fully restorable)
Encryption/Decryption (SSL/TLS)
```

### 🔹 Application Layer (L7) — Protocol Port Table 🔑

| Protocol | Port | Purpose |
|---|---|---|
| HTTP | 80 | Web pages (stateless) |
| HTTPS | 443 | Encrypted web (TLS) |
| DNS | 53 | Domain → IP |
| TELNET | 23 | Remote CLI access |
| DHCP | 67/68 | Auto IP assignment |
| FTP | 20 (data) / 21 (control) | File transfer |
| SMTP | 25 / 587 | Push email |
| POP3 | 110 | Pull email (offline) |
| IMAP | — | Sync email (multi-device) |
| NFS | 2049 | Remote file system mount |

### 🔹 HTTP Methods
```
GET     → fetch data
POST    → submit/create data
PUT     → update/replace
DELETE  → remove resource
HEAD    → headers only
CONNECT → tunnel (SSL)
```

### 🔹 DNS Resolution

```
Recursive Query                    Iterative Query
(resolver does ALL the work,       (resolver asks each server,
 client gets final IP only)         gets pointers, follows them)

Browser → Resolver → Root → TLD(.com) → Authoritative → IP
                ▲___________________________________│
                (caches result for next time)
```

### 🔹 Persistent vs Non-Persistent HTTP

```
Persistent (HTTP/1.1):  [Connection]──1 RTT──▶ fetch ALL objects (reused)
Non-Persistent(1.0):    [Connection]──2 RTT──▶ per object (new conn each time)
```

---

## 8. Delays

```
Total Delay = Transmission + Propagation + Processing + Queuing
                  (Tt)          (Tp)          (Tpro)      (Tq)
```

| Delay | Formula | Depends On |
|---|---|---|
| **Transmission (Tt)** | `L / B` (packet size / bandwidth) | Bandwidth, packet size |
| **Propagation (Tp)** | `Distance / Velocity` | Physical distance, medium |
| **Queuing (Tq)** | no formula | Traffic burstiness |
| **Processing (Tpro)** | no formula | CPU/router speed |

🧠 **Interview Byte:** *Congestion* → affects **Queuing Delay** the most.

---

## 9. 💻 Code Lab (C++ & Java)

> Beginner-friendly. Run these to actually **see** the Data Link Layer algorithms work.

### 9.1 Even Parity Bit Check (Error Detection)

**Logic:** Count 1s in data. If odd → add parity bit `1` to make total even. Receiver re-checks; mismatch = error.

```cpp
// parity_check.cpp
#include <iostream>
#include <string>
using namespace std;

int countOnes(string data) {
    int count = 0;
    for (char bit : data) if (bit == '1') count++;
    return count;
}

int main() {
    string data = "1011001"; // sample data bits
    int ones = countOnes(data);

    // SENDER SIDE: generate parity bit
    char parityBit = (ones % 2 == 0) ? '0' : '1';
    string transmitted = data + parityBit;
    cout << "Sent data with parity: " << transmitted << endl;

    // RECEIVER SIDE: check parity (simulate no error)
    int totalOnes = countOnes(transmitted);
    if (totalOnes % 2 == 0)
        cout << "✅ No error detected (even parity holds)" << endl;
    else
        cout << "❌ Error detected!" << endl;

    return 0;
}
```

```java
// ParityCheck.java
public class ParityCheck {

    static int countOnes(String data) {
        int count = 0;
        for (char bit : data.toCharArray())
            if (bit == '1') count++;
        return count;
    }

    public static void main(String[] args) {
        String data = "1011001"; // sample data bits
        int ones = countOnes(data);

        // SENDER SIDE: generate parity bit
        char parityBit = (ones % 2 == 0) ? '0' : '1';
        String transmitted = data + parityBit;
        System.out.println("Sent data with parity: " + transmitted);

        // RECEIVER SIDE: check parity (simulate no error)
        int totalOnes = countOnes(transmitted);
        if (totalOnes % 2 == 0)
            System.out.println("✅ No error detected (even parity holds)");
        else
            System.out.println("❌ Error detected!");
    }
}
```

---

### 9.2 Checksum (Simple Sum-Based)

**Logic:** Break data into blocks → sum them → 1's complement → that's the checksum. Receiver adds blocks + checksum → result should be all 1s (zero after complement) if no error.

```cpp
// checksum.cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> data = {12, 24, 8, 17}; // sample 8-bit-like blocks
    int sum = 0;
    for (int val : data) sum += val;

    // wrap carry (simulate 8-bit overflow)
    while (sum > 255) sum = (sum & 255) + (sum >> 8);

    int checksum = ~sum & 255; // 1's complement, kept to 8 bits
    cout << "Sum: " << sum << " | Checksum sent: " << checksum << endl;

    // RECEIVER: add data + checksum, result should be 255 (all 1s)
    int received = sum + checksum;
    cout << ((received == 255) ? "✅ No error" : "❌ Error detected") << endl;
    return 0;
}
```

```java
// Checksum.java
public class Checksum {
    public static void main(String[] args) {
        int[] data = {12, 24, 8, 17}; // sample 8-bit-like blocks
        int sum = 0;
        for (int val : data) sum += val;

        // wrap carry (simulate 8-bit overflow)
        while (sum > 255) sum = (sum & 255) + (sum >> 8);

        int checksum = (~sum) & 255; // 1's complement, kept to 8 bits
        System.out.println("Sum: " + sum + " | Checksum sent: " + checksum);

        // RECEIVER: add data + checksum, result should be 255 (all 1s)
        int received = sum + checksum;
        System.out.println(received == 255 ? "✅ No error" : "❌ Error detected");
    }
}
```

---

### 9.3 CRC (Cyclic Redundancy Check) — XOR Division

**Logic:** Append `(n-1)` zero bits (n = generator length) → do binary XOR division → remainder = CRC code appended to data.

```cpp
// crc.cpp
#include <iostream>
#include <string>
using namespace std;

string xorOp(string a, string b) {
    string result = "";
    for (size_t i = 1; i < a.size(); i++)
        result += (a[i] == b[i]) ? '0' : '1';
    return result;
}

string mod2div(string dividend, string divisor) {
    int pick = divisor.size();
    string temp = dividend.substr(0, pick);

    while (pick < (int)dividend.size()) {
        if (temp[0] == '1')
            temp = xorOp(divisor, temp) + dividend[pick];
        else
            temp = xorOp(string(pick, '0'), temp) + dividend[pick];
        pick += 1;
    }

    if (temp[0] == '1') temp = xorOp(divisor, temp);
    else temp = xorOp(string(pick - (int)dividend.size() + pick, '0'), temp);

    return temp.substr(1); // remainder = CRC
}

int main() {
    string data = "1101011111"; // original data
    string generator = "10011";  // generator polynomial

    string appended = data + string(generator.size() - 1, '0');
    string remainder = mod2div(appended, generator);

    cout << "Data: " << data << endl;
    cout << "CRC remainder: " << remainder << endl;
    cout << "Transmitted (data+CRC): " << data + remainder << endl;
    return 0;
}
```

```java
// CRC.java
public class CRC {

    static String xorOp(String a, String b) {
        StringBuilder result = new StringBuilder();
        for (int i = 1; i < a.length(); i++)
            result.append(a.charAt(i) == b.charAt(i) ? '0' : '1');
        return result.toString();
    }

    static String mod2div(String dividend, String divisor) {
        int pick = divisor.length();
        String temp = dividend.substring(0, pick);

        while (pick < dividend.length()) {
            if (temp.charAt(0) == '1')
                temp = xorOp(divisor, temp) + dividend.charAt(pick);
            else
                temp = xorOp("0".repeat(pick), temp) + dividend.charAt(pick);
            pick += 1;
        }

        if (temp.charAt(0) == '1') temp = xorOp(divisor, temp);

        return temp.substring(1); // remainder = CRC
    }

    public static void main(String[] args) {
        String data = "1101011111"; // original data
        String generator = "10011";  // generator polynomial

        String appended = data + "0".repeat(generator.length() - 1);
        String remainder = mod2div(appended, generator);

        System.out.println("Data: " + data);
        System.out.println("CRC remainder: " + remainder);
        System.out.println("Transmitted (data+CRC): " + data + remainder);
    }
}
```

---

### 9.4 Find IP Class + Network/Broadcast/Subnet Mask

**Logic:** Look at the first octet → decide class A/B/C/D/E → apply default mask → AND with IP to get network address.

```cpp
// ip_class.cpp
#include <iostream>
#include <sstream>
#include <vector>
using namespace std;

int main() {
    string ip = "192.168.1.20"; // sample IP
    stringstream ss(ip);
    string segment;
    vector<int> octets;
    while (getline(ss, segment, '.')) octets.push_back(stoi(segment));

    int first = octets[0];
    string ipClass;
    string mask;

    if (first <= 127)      { ipClass = "A"; mask = "255.0.0.0"; }
    else if (first <= 191) { ipClass = "B"; mask = "255.255.0.0"; }
    else if (first <= 223) { ipClass = "C"; mask = "255.255.255.0"; }
    else if (first <= 239) { ipClass = "D (Multicast)"; mask = "N/A"; }
    else                    { ipClass = "E (Reserved)"; mask = "N/A"; }

    cout << "IP: " << ip << endl;
    cout << "Class: " << ipClass << endl;
    cout << "Default Subnet Mask: " << mask << endl;
    return 0;
}
```

```java
// IPClass.java
public class IPClass {
    public static void main(String[] args) {
        String ip = "192.168.1.20"; // sample IP
        String[] octets = ip.split("\\.");
        int first = Integer.parseInt(octets[0]);

        String ipClass, mask;

        if (first <= 127)      { ipClass = "A"; mask = "255.0.0.0"; }
        else if (first <= 191) { ipClass = "B"; mask = "255.255.0.0"; }
        else if (first <= 223) { ipClass = "C"; mask = "255.255.255.0"; }
        else if (first <= 239) { ipClass = "D (Multicast)"; mask = "N/A"; }
        else                   { ipClass = "E (Reserved)"; mask = "N/A"; }

        System.out.println("IP: " + ip);
        System.out.println("Class: " + ipClass);
        System.out.println("Default Subnet Mask: " + mask);
    }
}
```

---

### 9.5 Subnetting Calculator (Subnets & Hosts from CIDR)

**Logic:** Given `/n`, host bits = `32 - n`. Hosts = `2^(host bits) - 2`. Borrowed bits → subnets = `2^borrowed`.

```cpp
// subnet_calc.cpp
#include <iostream>
#include <cmath>
using namespace std;

int main() {
    int cidr = 28; // example: /28
    int hostBits = 32 - cidr;
    long long usableHosts = (long long)pow(2, hostBits) - 2;

    cout << "CIDR: /" << cidr << endl;
    cout << "Host bits: " << hostBits << endl;
    cout << "Usable Hosts: " << usableHosts << endl;

    // Example: Class C (/24) borrowing 2 bits -> /26
    int borrowedBits = 2;
    long long subnets = (long long)pow(2, borrowedBits);
    int remainingHostBits = 8 - borrowedBits; // class C has 8 host bits
    long long hostsPerSubnet = (long long)pow(2, remainingHostBits) - 2;

    cout << "\nBorrowing " << borrowedBits << " bits from Class C:" << endl;
    cout << "Subnets created: " << subnets << endl;
    cout << "Hosts per subnet: " << hostsPerSubnet << endl;
    return 0;
}
```

```java
// SubnetCalc.java
public class SubnetCalc {
    public static void main(String[] args) {
        int cidr = 28; // example: /28
        int hostBits = 32 - cidr;
        long usableHosts = (long) Math.pow(2, hostBits) - 2;

        System.out.println("CIDR: /" + cidr);
        System.out.println("Host bits: " + hostBits);
        System.out.println("Usable Hosts: " + usableHosts);

        // Example: Class C (/24) borrowing 2 bits -> /26
        int borrowedBits = 2;
        long subnets = (long) Math.pow(2, borrowedBits);
        int remainingHostBits = 8 - borrowedBits; // class C has 8 host bits
        long hostsPerSubnet = (long) Math.pow(2, remainingHostBits) - 2;

        System.out.println("\nBorrowing " + borrowedBits + " bits from Class C:");
        System.out.println("Subnets created: " + subnets);
        System.out.println("Hosts per subnet: " + hostsPerSubnet);
    }
}
```

---

### 9.6 Stop-and-Wait Protocol Simulation

**Logic:** Sender sends 1 frame → waits for ACK → only then sends next. Simulates idle time + lost-frame timeout.

```cpp
// stop_and_wait.cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> frames = {0, 1, 2, 3, 4};
    vector<bool> lost = {false, false, true, false, false}; // frame 2 will "get lost"

    for (size_t i = 0; i < frames.size(); i++) {
        cout << "Sender -> Frame " << frames[i] << " sent." << endl;

        if (lost[i]) {
            cout << "   ⚠️  Frame lost in transit! Timeout occurs..." << endl;
            cout << "   🔁 Sender retransmits Frame " << frames[i] << endl;
        }
        cout << "Receiver -> ACK " << frames[i] << " received. Sender can proceed.\n" << endl;
    }
    return 0;
}
```

```java
// StopAndWait.java
public class StopAndWait {
    public static void main(String[] args) {
        int[] frames = {0, 1, 2, 3, 4};
        boolean[] lost = {false, false, true, false, false}; // frame 2 will "get lost"

        for (int i = 0; i < frames.length; i++) {
            System.out.println("Sender -> Frame " + frames[i] + " sent.");

            if (lost[i]) {
                System.out.println("   ⚠️  Frame lost in transit! Timeout occurs...");
                System.out.println("   🔁 Sender retransmits Frame " + frames[i]);
            }
            System.out.println("Receiver -> ACK " + frames[i] + " received. Sender can proceed.\n");
        }
    }
}
```

---

## 10. 🎯 Placement Interview Question Bank

> ⭐ = asked very frequently in product-based company interviews

1. ⭐ Walk me through what happens when you type `www.google.com` and hit Enter.
2. ⭐ Explain the TCP 3-way handshake. What if the final ACK is lost?
3. ⭐ Difference between TCP and UDP — when would you choose each?
4. ⭐ Difference between a Hub, Switch, and Router (with OSI layer).
5. What is the difference between a Router and a Switch?
6. ⭐ What is DNS? Difference between recursive and iterative queries?
7. Explain Go-Back-N vs Selective Repeat ARQ.
8. ⭐ Why do we need both MAC and IP addresses?
9. What is subnetting, and why is it useful?
10. ⭐ Explain how a firewall protects a network. Can it stop internal threats?
11. Difference between Persistent and Non-Persistent HTTP.
12. What is NAT and why is it needed?
13. ⭐ Difference between CSMA/CD and CSMA/CA — why is CD not used in modern wired LANs?
14. Explain the 4 types of network delay.
15. What is the difference between IPv4 and IPv6 headers?
16. Explain Client-Server vs Peer-to-Peer with real examples.
17. What's the difference between Unicast, Multicast, Broadcast, Anycast?
18. ⭐ Explain how ARP resolves IP to MAC address.
19. What is a VPN and how does it provide security?
20. ⭐ What is the Count-to-Infinity problem in RIP, and how is it solved (split horizon, etc.)?
21. Explain how Hamming code detects AND corrects errors (forward error correction).
22. What is piggybacking in TCP?
23. Difference between Stop-and-Wait, Go-Back-N, and Selective Repeat.
24. What's the role of the Session Layer vs Presentation Layer?
25. Explain CIDR notation and how to calculate the number of usable hosts.

🧠 **Quick-fire answers:**

| Q | One-liner answer |
|---|---|
| OSI data units? | Bits → Frames → Packets → Segments → Data (L1→L4→ up) |
| Protocol for reliable transfer? | TCP |
| Layer of Router? | Network (L3) |
| Layer of Switch? | Data Link (L2) |
| Stateless protocol? | HTTP |
| Connectionless transport protocol? | UDP |

---

## 11. ✅ MCQ Sprint + Answer Key

1. Reliable communication protocol? → **C. TCP**
2. File transfer protocol? → **B. FTP**
3. Single room network? → **A. PAN**
4. Campus-wide network? → **C. CAN**
5. Centralized + secure architecture? → **B. Client-Server**
6. Topology with central device? → **C. Star**
7. Full redundancy topology? → **A. Mesh**
8. Device at Data Link Layer? → **B. Switch**
9. Router's job? → **B. Forwards packets between networks**
10. First step of TCP handshake? → **A. SYN**
11. If last ACK lost? → **D. Sender waits / times out**
12. Delay from pushing data into link? → **C. Transmission**
13. Delay most affected by congestion? → **C. Queuing**
14. IPv6 address size? → **C. 128-bit**
15. Can private IP access internet directly? → **B. No (needs NAT)**
16. First step on visiting a website? → **B. DNS resolution**
17. One-to-many communication? → **B. Multicast**
18. VPN provides? → **B. Encrypted remote access**
19. Network layer data unit? → **C. Packet**
20. DNS converts? → **B. Domain → IP**
21. MAC address unique to? → **C. Device**
22. Flow control prevents? → **A. Packet loss from congestion**
23. Protocol with no flow/error control? → **B. UDP**
24. Session layer responsible for? → **C. Dialog control**

> 📚 *Full 35-question MCQ set + answer key available in the original PDF (page 55–57) if you want extra practice.*

---

### 🏁 Final Tip
For interviews, always **explain the "why"**, not just the "what" — e.g., not just *"TCP uses 3-way handshake"* but *"TCP uses it to synchronize sequence numbers on both ends so reliable, ordered delivery is possible."* That depth is what separates a 6/10 answer from a 10/10 answer. Good luck! 🚀