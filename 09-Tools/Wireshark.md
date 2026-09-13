# Wireshark

> Graphical packet capture, protocol inspection, traffic filtering, and network analysis for authorized security assessments and troubleshooting.

## What is Wireshark?

**Wireshark** is a graphical network protocol analyzer.

It captures or opens network packets and decodes hundreds of protocols.

Conceptually:

```text
Network Interface / PCAP
          ↓
      Wireshark
          ↓
    Packet Decoding
          ↓
 Protocol Analysis
```

It is one of the most important tools for understanding what is actually happening on a network.

---

# What Can Wireshark Analyze?

Examples include:

```text
Ethernet
ARP
IPv4 / IPv6
ICMP
TCP
UDP
DNS
DHCP
HTTP
TLS
SMB
Kerberos
LDAP
SSH metadata
Many other protocols
```

---

# Packet Structure

A network packet can contain multiple protocol layers.

Conceptually:

```text
Ethernet
   ↓
IP
   ↓
TCP
   ↓
HTTP
```

Wireshark separates these layers for inspection.

Example:

```text
Frame
 |
 +-- Ethernet II
 |
 +-- Internet Protocol
 |
 +-- Transmission Control Protocol
 |
 +-- Hypertext Transfer Protocol
```

---

# OSI Perspective

Wireshark helps visualize networking layers.

```text
7 Application    HTTP / DNS / SMB
6 Presentation   Encoding / TLS concepts
5 Session        Session-related protocols
4 Transport      TCP / UDP
3 Network        IP
2 Data Link      Ethernet / MAC
1 Physical       Transmission medium
```

The practical focus is usually on:

```text
Layer 2
Layer 3
Layer 4
Layer 7
```

---

# Capture Interface

Before capturing, select an interface.

Examples:

```text
Ethernet
Wi-Fi
Loopback
VPN
Virtual adapter
```

Conceptually:

```text
Interface
    ↓
Packets
    ↓
Wireshark
```

Choosing the wrong interface may produce no relevant traffic.

---

# Live Capture

Wireshark can capture packets directly from an interface.

Workflow:

```text
Select Interface
      ↓
Start Capture
      ↓
Generate / Observe Traffic
      ↓
Stop Capture
      ↓
Analyze
```

Packet capture may require elevated privileges depending on the operating system and configuration.

---

# PCAP Files

Wireshark can open:

```text
.pcap
.pcapng
```

capture files.

A common workflow:

```text
Remote Server
     ↓
tcpdump
     ↓
capture.pcap
     ↓
Wireshark
     ↓
Detailed Analysis
```

---

# Tcpdump vs Wireshark

```text
Tcpdump
   |
CLI
   |
Fast capture
   |
Server friendly
```

versus:

```text
Wireshark
   |
GUI
   |
Deep protocol decoding
   |
Interactive analysis
```

They complement each other.

---

# Packet List Pane

Wireshark typically displays packets with fields such as:

```text
Number
Time
Source
Destination
Protocol
Length
Info
```

Example:

```text
10.0.0.5 → 10.0.0.10
TCP
443
```

---

# Packet Details

Selecting a packet exposes protocol layers.

Example:

```text
Frame
Ethernet
IPv4
TCP
TLS
```

Expand each section to inspect individual fields.

---

# Packet Bytes

Wireshark also displays raw packet bytes.

Conceptually:

```text
Raw Bytes
    ↕
Decoded Protocol Fields
```

This is useful when protocol decoding or exact byte values matter.

---

# Display Filters

One of Wireshark's most powerful features is:

```text
Display Filters
```

These control which already-captured packets are shown.

They do not necessarily change the contents of the capture file.

---

# Filter by Protocol

TCP:

```text
tcp
```

UDP:

```text
udp
```

DNS:

```text
dns
```

HTTP:

```text
http
```

TLS:

```text
tls
```

ARP:

```text
arp
```

ICMP:

```text
icmp
```

---

# Filter by IP

Example:

```text
ip.addr == 10.0.0.10
```

Shows packets where that address is either source or destination.

Source only:

```text
ip.src == 10.0.0.10
```

Destination:

```text
ip.dst == 10.0.0.10
```

---

# Filter by TCP Port

```text
tcp.port == 443
```

Source port:

```text
tcp.srcport == 443
```

Destination:

```text
tcp.dstport == 443
```

---

# Filter by UDP Port

Example:

```text
udp.port == 53
```

Useful when examining traditional DNS traffic over UDP.

---

# Logical Operators

Filters can be combined.

AND:

```text
ip.addr == 10.0.0.10 && tcp.port == 443
```

OR:

```text
tcp.port == 80 || tcp.port == 443
```

NOT:

```text
!arp
```

This makes complex traffic isolation possible.

---

# Capture Filters vs Display Filters

Critical distinction:

## Capture Filter

Determines:

```text
What gets captured
```

Often uses BPF-style syntax.

Example concept:

```text
port 443
```

## Display Filter

Determines:

```text
What already-captured packets
are displayed
```

Example:

```text
tcp.port == 443
```

The syntaxes are different.

---

# DNS Analysis

Filter:

```text
dns
```

DNS flow:

```text
Client
  |
Query
  |
  v
DNS Server
  |
Response
  |
  v
Client
```

Wireshark can show:

```text
Query name
Query type
Response
Resolved address
Error codes
```

---

# DNS Record Types

Common:

```text
A       IPv4
AAAA    IPv6
CNAME   Alias
MX      Mail server
NS      Name server
TXT     Text
```

DNS analysis is useful for both troubleshooting and security investigations.

---

# TCP Handshake

TCP connection establishment:

```text
Client                  Server

SYN ------------------->

    <--------------- SYN-ACK

ACK ------------------->
```

Wireshark displays TCP flags and sequence information.

---

# TCP Flags

Important:

```text
SYN
ACK
FIN
RST
PSH
```

These help explain connection state.

---

# RST

A TCP reset:

```text
RST
```

can indicate:

```text
Rejected connection
Application termination
Closed port
Protocol issue
Firewall behavior
```

Context is required.

---

# TCP Retransmissions

Wireshark may identify:

```text
TCP Retransmission
```

Potential causes include:

```text
Packet loss
Network congestion
Latency
Receiver problems
```

This makes Wireshark useful beyond security testing.

---

# Follow TCP Stream

One of the most useful analysis features is:

```text
Follow TCP Stream
```

Conceptually:

```text
Many individual packets
        ↓
Reconstruct conversation
        ↓
Readable client/server stream
```

This is especially useful for plaintext protocols.

---

# HTTP

Filter:

```text
http
```

Plain HTTP can expose:

```text
Methods
URLs
Headers
Cookies
Request bodies
Responses
```

Example:

```http
GET /login HTTP/1.1
Host: example.test
```

---

# Plaintext Credentials

Legacy/plaintext protocols can expose sensitive data directly in packet captures.

Examples historically include:

```text
HTTP
FTP
Telnet
Some mail configurations
```

This demonstrates why encryption in transit is critical.

---

# HTTPS / TLS

HTTPS:

```text
HTTP
  ↓
TLS Encryption
  ↓
TCP
```

Wireshark can normally observe:

```text
Source/Destination
TLS handshake
Certificate information
Packet sizes
Timing
Protocol versions
```

but not application plaintext unless suitable decryption material is legitimately available.

---

# TLS Handshake

Simplified:

```text
Client
   |
Client Hello
   ↓
Server
   |
Server Hello
Certificate
...
   ↓
Encrypted Session
```

Wireshark can help inspect TLS negotiation details.

---

# TLS Certificates

Certificate analysis may reveal:

```text
Issuer
Subject
Validity period
Subject Alternative Names
Signature algorithm
```

Useful for:

```text
TLS troubleshooting
Certificate validation
Infrastructure mapping
```

---

# ARP

Filter:

```text
arp
```

ARP maps:

```text
IPv4 Address
      ↓
MAC Address
```

Example:

```text
Who has 192.168.1.1?
```

Response:

```text
192.168.1.1 is at aa:bb:cc:dd:ee:ff
```

---

# ICMP

Filter:

```text
icmp
```

Ping concept:

```text
Echo Request
     ↓
Target
     ↓
Echo Reply
```

Useful for network troubleshooting.

---

# DHCP

DHCP provides automatic network configuration.

Conceptually:

```text
Client
  |
Discover
  ↓
Server
  |
Offer
  ↓
Client
  |
Request
  ↓
Server
  |
ACK
```

Often remembered as:

```text
DORA

Discover
Offer
Request
Acknowledge
```

Wireshark can display this process.

---

# SMB

Wireshark can decode SMB traffic.

SMB is commonly associated with:

```text
Windows file sharing
Network shares
Domain environments
Remote administration
```

Modern SMB traffic may include encryption/signing depending on configuration, affecting what is visible.

---

# Kerberos

In Active Directory environments, Wireshark can inspect Kerberos protocol exchanges.

Common conceptual flow:

```text
AS-REQ
 ↓
AS-REP
 ↓
TGS-REQ
 ↓
TGS-REP
```

This makes Wireshark valuable for understanding AD authentication at the protocol level.

---

# LDAP

LDAP traffic can also be analyzed.

Conceptually:

```text
Client
  |
LDAP
  |
  v
Directory Server
```

Encrypted LDAP/TLS limits plaintext visibility.

---

# Network Conversations

Wireshark can summarize conversations between:

```text
IP addresses
TCP endpoints
UDP endpoints
```

This helps answer:

```text
Which hosts communicated?
How much data?
Which ports?
```

---

# Endpoints

Endpoint statistics help identify:

```text
Active IP addresses
MAC addresses
Traffic volume
```

Useful when analyzing unfamiliar PCAP files.

---

# Protocol Hierarchy

Protocol statistics can answer:

```text
What protocols dominate this capture?
```

Example:

```text
Ethernet
 └── IPv4
      ├── TCP
      │    ├── TLS
      │    └── HTTP
      └── UDP
           └── DNS
```

---

# Security Investigation Workflow

Suppose suspicious network activity is reported.

Use:

```text
PCAP
 ↓
Identify Endpoints
 ↓
Identify Protocols
 ↓
Filter Suspicious Host
 ↓
Analyze Conversations
 ↓
Follow Streams
 ↓
Build Timeline
 ↓
Correlate With Endpoint Logs
```

---

# Suspicious Traffic

Potential indicators can include:

```text
Unexpected external connections
Unusual DNS queries
Rare ports
Periodic beacon-like traffic
Large outbound transfers
Unexpected protocols
Connections to unknown infrastructure
```

None of these alone proves malicious activity.

---

# Beaconing Concept

Command-and-control traffic may sometimes appear periodic:

```text
Host
 |
Connection
 |
wait
 |
Connection
 |
wait
 |
Connection
```

Regular timing can be an investigative clue.

But legitimate applications also generate periodic traffic.

---

# Exfiltration Analysis

Network analysis may help identify:

```text
Large outbound transfers
Unexpected destinations
Unusual protocols
Abnormal DNS activity
```

Always correlate with application/endpoint evidence.

---

# PCAP Security

Packet captures may contain highly sensitive information:

```text
Credentials
Cookies
Tokens
Internal IPs
Usernames
Emails
DNS history
Application data
```

Therefore:

```text
PCAP
=
Sensitive Evidence
```

Protect it appropriately.

---

# Large PCAP Strategy

Do not manually inspect millions of packets sequentially.

Use:

```text
Statistics
 ↓
Endpoints
 ↓
Conversations
 ↓
Protocols
 ↓
Filters
 ↓
Interesting Streams
```

This dramatically reduces analysis time.

---

# Useful Filters Cheat Sheet

Protocol:

```text
dns
http
tls
tcp
udp
icmp
arp
```

IP:

```text
ip.addr == 10.0.0.10
```

Source:

```text
ip.src == 10.0.0.10
```

Destination:

```text
ip.dst == 10.0.0.10
```

Port:

```text
tcp.port == 443
```

DNS query:

```text
dns
```

HTTP request:

```text
http.request
```

TCP SYN packets:

```text
tcp.flags.syn == 1
```

---

# Analysis Methodology

```text
1. Understand capture source

2. Identify capture timeframe

3. Review protocol hierarchy

4. Review endpoints

5. Review conversations

6. Identify unusual hosts

7. Apply display filters

8. Follow relevant streams

9. Build network timeline

10. Correlate with system/application logs
```

---

# Wireshark vs Burp

Both inspect network/application communication but at different layers.

```text
Burp / Caido
    |
HTTP application testing
    |
Request modification
```

versus:

```text
Wireshark
    |
Packet/protocol analysis
    |
Network visibility
```

Burp is generally better for interactive web application testing.

Wireshark is better for observing lower-level protocol traffic.

---

# Wireshark vs Tcpdump

```text
Tcpdump
   ↓
Capture quickly
   ↓
CLI
```

```text
Wireshark
   ↓
Analyze deeply
   ↓
GUI
```

Excellent combined workflow:

```text
tcpdump
   ↓
capture.pcap
   ↓
Wireshark
```

---

# Key Takeaway

Wireshark's mental model is:

```text
PACKETS
   ↓
PROTOCOL DECODING
   ↓
FILTERING
   ↓
CONVERSATIONS
   ↓
STREAMS
   ↓
ANALYSIS
```

Do not begin by staring at individual packets.

Start with:

```text
Who communicated?

Using which protocol?

On which ports?

How much data?

At what time?

What happened inside the relevant conversation?
```

Then progressively narrow the capture.

Wireshark's core value is turning:

```text
Raw Network Traffic
```

into:

```text
Understandable Protocol Behavior.
```
