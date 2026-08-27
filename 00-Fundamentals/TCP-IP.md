# 📡 TCP/IP Model

> The TCP/IP model defines how data travels from one computer to another across a network. Every penetration tester must understand this model because almost every attack relies on one or more TCP/IP protocols.

---

# Overview

The **TCP/IP (Transmission Control Protocol / Internet Protocol)** model is the networking model used on the Internet.

Whenever you:

- Open a website
- Connect through SSH
- Use Burp Suite
- Scan with Nmap
- Use Metasploit

...you are using the TCP/IP stack.

---

# Learning Objectives

After this chapter you should understand:

- TCP/IP architecture
- TCP/IP Layers
- Encapsulation
- Packet Flow
- TCP vs UDP
- Three-Way Handshake
- Common Protocols
- Why attackers care about TCP/IP

---

# What is TCP/IP?

TCP/IP is a collection of networking protocols that define how computers communicate.

Think of TCP/IP as a language spoken by every device connected to the Internet.

---

# TCP/IP Layers

The TCP/IP model has **4 layers**.

```text
+----------------------+
| Application Layer    |
+----------------------+
| Transport Layer      |
+----------------------+
| Internet Layer       |
+----------------------+
| Network Access Layer |
+----------------------+
```

---

# 1. Application Layer

Responsible for user-facing protocols.

Examples

- HTTP
- HTTPS
- FTP
- SMTP
- DNS
- SSH
- SMB

Example

When you visit

```
https://google.com
```

HTTP/HTTPS operates here.

---

# 2. Transport Layer

Responsible for communication between applications.

Protocols

- TCP
- UDP

Responsibilities

- Segmentation
- Reliability
- Flow Control
- Error Detection

---

# TCP

TCP provides

- Reliable communication
- Ordered delivery
- Error checking
- Retransmission

Examples

- HTTP
- HTTPS
- SSH
- FTP
- SMTP

TCP is slower but reliable.

---

# UDP

UDP provides

- Fast communication
- No connection
- No guarantee of delivery

Examples

- DNS
- VoIP
- Streaming
- Gaming

UDP is faster but unreliable.

---

# TCP vs UDP

| TCP | UDP |
|------|-----|
| Connection-Oriented | Connectionless |
| Reliable | Unreliable |
| Slower | Faster |
| Ordered | Unordered |
| Error Recovery | No Recovery |

---

# 3. Internet Layer

Responsible for routing packets.

Protocols

- IPv4
- IPv6
- ICMP

Example

```
192.168.1.10
```

belongs here.

---

# 4. Network Access Layer

Responsible for physical communication.

Examples

- Ethernet
- Wi-Fi

Uses MAC Addresses.

---

# Encapsulation

Every layer adds its own information.

```text
HTTP Data

↓

TCP Header

↓

IP Header

↓

Ethernet Header

↓

Bits
```

On the receiving side, the process is reversed.

---

# TCP Three-Way Handshake

Before communication begins:

```text
Client
   |
   | SYN
   |
Server
   |
   | SYN-ACK
   |
Client
   |
   | ACK
   |
Connection Established
```

This is called the **Three-Way Handshake**.

---

# Four-Way Termination

TCP closes connections using:

```text
FIN

ACK

FIN

ACK
```

---

# Common Protocols

| Protocol | Port |
|-----------|-----:|
| HTTP | 80 |
| HTTPS | 443 |
| FTP | 21 |
| SSH | 22 |
| DNS | 53 |
| SMTP | 25 |
| POP3 | 110 |
| IMAP | 143 |
| SMB | 445 |
| LDAP | 389 |
| RDP | 3389 |

---

# Packet Flow Example

Opening a website:

```text
Browser

↓

DNS Lookup

↓

TCP Handshake

↓

TLS Handshake (HTTPS)

↓

HTTP Request

↓

HTTP Response

↓

Browser Renders Page
```

---

# Useful Commands

View IP

```bash
ip a
```

View Routing Table

```bash
ip route
```

Ping Host

```bash
ping google.com
```

Traceroute

```bash
traceroute google.com
```

Open Connections

```bash
ss -tuln
```

Capture Packets

```bash
tcpdump -i eth0
```

---

# Red Team Perspective

Understanding TCP/IP helps with:

- Network Scanning
- Firewall Bypass
- Packet Analysis
- Traffic Interception
- Pivoting
- Reverse Shells
- Port Forwarding
- Active Directory Enumeration

Without TCP/IP, tools like:

- Nmap
- Wireshark
- Burp Suite
- Metasploit

make little sense.

---

# Common Beginner Mistakes

- Confusing TCP with IP
- Confusing IP addresses with Ports
- Thinking TCP and HTTP are the same
- Forgetting the TCP Handshake
- Ignoring UDP services

---

# Interview Questions

### What is TCP/IP?

### Difference between TCP and UDP?

### What is a Three-Way Handshake?

### Why is TCP reliable?

### Why is UDP faster?

### What happens before an HTTP request?

### Which layer handles routing?

---

# Quick Revision

- TCP/IP has **4 layers**
- TCP = Reliable
- UDP = Fast
- IP handles routing
- Ethernet/Wi-Fi operate at the Network Access Layer
- TCP uses a Three-Way Handshake
- Encapsulation adds headers at each layer
- HTTP runs over TCP

---

# Practice

TryHackMe

- Network Fundamentals
- Intro to Networking

Hack The Box Academy

- Introduction to Networking

Cisco Networking Academy

- Introduction to Networks

---

# References

- RFC 793 (TCP)
- RFC 791 (IPv4)
- RFC 768 (UDP)
- Cisco Networking Academy
- Hack The Box Academy
- TryHackMe
