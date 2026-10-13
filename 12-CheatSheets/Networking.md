
# Networking — Cheat Sheet

> Essential networking concepts, protocols, ports, diagnostics, and packet-analysis commands for cybersecurity.

## 1. OSI Model

| Layer | Name | Examples |
|---|---|---|
| 7 | Application | HTTP, DNS, SMTP |
| 6 | Presentation | Encoding, serialization, TLS-related functions |
| 5 | Session | Session management |
| 4 | Transport | TCP, UDP |
| 3 | Network | IP, ICMP |
| 2 | Data Link | Ethernet, MAC |
| 1 | Physical | Cables, radio, signals |

Remember:

```text
Application
Presentation
Session
Transport
Network
Data Link
Physical
```

---

## 2. TCP/IP Model

```text
Application
    ↓
Transport
    ↓
Internet
    ↓
Link
```

Example:

```text
HTTP
 ↓
TCP
 ↓
IP
 ↓
Ethernet
```

---

## 3. TCP vs UDP

| TCP | UDP |
|---|---|
| Connection-oriented | Connectionless |
| Reliable ordered byte stream | No built-in delivery guarantee |
| Retransmission | No automatic retransmission |
| Flow/congestion control | Application-dependent controls |
| HTTP/1.1, SSH | DNS, VoIP, QUIC |

---

## 4. TCP Three-Way Handshake

```text
Client                 Server

SYN ------------------>

     <------------- SYN-ACK

ACK ------------------>
```

Purpose:

```text
Establish TCP connection
Synchronize sequence numbers
```

---

## 5. TCP Flags

```text
SYN = Synchronize
ACK = Acknowledgment
FIN = Finish
RST = Reset
PSH = Push
URG = Urgent
```

Common observations:

```text
SYN       Connection initiation
SYN-ACK   Response to SYN
RST       Connection reset
FIN       Graceful close
```

---

## 6. IP Addressing

IPv4 example:

```text
192.168.1.10
```

IPv6 example:

```text
2001:db8::1
```

Private IPv4 ranges:

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

Loopback:

```text
127.0.0.1
::1
```

---

## 7. CIDR

CIDR specifies network prefixes.

Examples:

```text
192.168.1.0/24
10.0.0.0/8
172.16.0.0/12
```

For IPv4 `/24`:

```text
Network bits: 24
Host bits: 8
Addresses: 256
```

A conventional `/24` LAN typically has 254 usable host addresses.

---

## 8. Common Ports

| Port | Service |
|---|---|
| 20/21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 67/68 | DHCP |
| 80 | HTTP |
| 88 | Kerberos |
| 110 | POP3 |
| 123 | NTP |
| 135 | RPC |
| 139 | NetBIOS |
| 143 | IMAP |
| 161 | SNMP |
| 389 | LDAP |
| 443 | HTTPS |
| 445 | SMB |
| 636 | LDAPS |
| 993 | IMAPS |
| 995 | POP3S |
| 1433 | MSSQL |
| 3306 | MySQL |
| 3389 | RDP |
| 5432 | PostgreSQL |
| 5985/5986 | WinRM |
| 6379 | Redis |
| 8080 | Alternate HTTP |

Ports indicate likely services, not guaranteed service identity.

---

## 9. DNS

**DNS = Domain Name System**

Common records:

```text
A       IPv4
AAAA    IPv6
CNAME   Alias
MX      Mail
NS      Nameserver
TXT     Text
SOA     Zone authority
PTR     Reverse lookup
SRV     Service discovery
```

Query:

```bash
dig example.com
```

Specific record:

```bash
dig example.com MX
```

Nameservers:

```bash
dig example.com NS
```

---

## 10. DHCP

**DHCP = Dynamic Host Configuration Protocol**

DORA:

```text
Discover
   ↓
Offer
   ↓
Request
   ↓
Acknowledge
```

Provides:

```text
IP address
Subnet mask
Gateway
DNS configuration
```

---

## 11. ARP

**ARP = Address Resolution Protocol**

Maps IPv4 addresses to MAC addresses on local links.

```text
IPv4
 ↓
ARP
 ↓
MAC Address
```

Linux:

```bash
ip neigh
```

macOS:

```bash
arp -a
```

---

## 12. ICMP

Used for network-control and diagnostic messages.

Ping:

```bash
ping -c 4 192.0.2.10
```

Useful for:

```text
Reachability
Latency
Packet loss
Error reporting
```

ICMP may be blocked even when the host is online.

---

## 13. Routing

Linux:

```bash
ip route
```

macOS:

```bash
netstat -rn
```

Default gateway:

```text
0.0.0.0/0
```

represents the default IPv4 route.

---

## 14. NAT

**NAT = Network Address Translation**

Conceptually:

```text
Private Network
       ↓
NAT Router
       ↓
Public Internet
```

Common use:

```text
Multiple private hosts
sharing a public IPv4 address
```

---

## 15. HTTP

Common methods:

```text
GET
POST
PUT
PATCH
DELETE
HEAD
OPTIONS
```

HTTP response categories:

```text
1xx Informational
2xx Success
3xx Redirect
4xx Client Error
5xx Server Error
```

---

## 16. HTTPS and TLS

HTTPS normally uses TLS to protect HTTP traffic.

Provides:

```text
Confidentiality
Integrity
Server authentication
```

Inspect TLS:

```bash
openssl s_client -connect example.com:443 -servername example.com
```

---

## 17. Network Diagnostics

DNS:

```bash
dig example.com
```

HTTP:

```bash
curl -I https://example.com
```

TCP connectivity:

```bash
nc -vz example.com 443
```

Route tracing on Linux:

```bash
traceroute example.com
```

macOS:

```bash
traceroute example.com
```

---

## 18. Listening Ports

Linux:

```bash
ss -tuln
```

Processes:

```bash
sudo ss -tulpn
```

macOS:

```bash
lsof -nP -iTCP -sTCP:LISTEN
```

---

## 19. Packet Capture

Tcpdump:

```bash
sudo tcpdump -nn -i en0
```

DNS:

```bash
sudo tcpdump -nn port 53
```

HTTP/HTTPS metadata:

```bash
sudo tcpdump -nn 'port 80 or port 443'
```

Save:

```bash
sudo tcpdump -i en0 -w capture.pcap
```

Replace `en0` with the appropriate interface.

---

## 20. Wireshark Filters

```text
dns
tcp
udp
http
tls
icmp
arp
```

IP:

```text
ip.addr == 192.0.2.10
```

Port:

```text
tcp.port == 443
```

DNS queries:

```text
dns.flags.response == 0
```

---

## 21. Network Security Concepts

Important technologies:

```text
Firewall
IDS
IPS
VPN
Proxy
Reverse Proxy
WAF
Network Segmentation
Zero Trust
```

### IDS

Detects suspicious activity.

### IPS

Can actively block traffic.

### WAF

Filters web-application traffic.

---

## 22. Network Troubleshooting Workflow

```text
Interface Up?
     ↓
Valid IP?
     ↓
Gateway Reachable?
     ↓
DNS Working?
     ↓
Port Reachable?
     ↓
TLS Working?
     ↓
Application Responding?
```

---

## Key Takeaway

Networking analysis should follow:

```text
IP
 ↓
Routing
 ↓
Transport
 ↓
Protocol
 ↓
Application
```

Always distinguish:

```text
Host Reachability
      !=
Port Reachability
      !=
Application Availability
```
