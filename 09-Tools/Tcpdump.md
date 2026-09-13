# Tcpdump

> Command-line packet capture and network-traffic analysis for authorized troubleshooting and security assessments.

## What is Tcpdump?

**tcpdump** is a command-line packet capture and network-analysis utility.

It captures traffic passing through network interfaces.

Conceptually:

```text
Network Interface
       ↓
     tcpdump
       ↓
     Packets
       ↓
Display / PCAP File
```

It is particularly useful on:

```text
Linux servers
Remote systems
Minimal environments
SSH sessions
Incident-response systems
```

---

# Packets

Network communication is transmitted as packets.

A packet may contain information such as:

```text
Source IP
Destination IP
Protocol
Source Port
Destination Port
Flags
Payload
```

tcpdump allows these packets to be observed.

---

# Interfaces

First identify available network interfaces:

```bash
ip addr
```

or:

```bash
tcpdump -D
```

Possible interfaces:

```text
eth0
ens33
en0
wlan0
lo
```

---

# Capture on an Interface

A common pattern:

```bash
sudo tcpdump -i eth0
```

This captures packets visible to `eth0`.

Capturing packets generally requires elevated network privileges.

---

# Any Interface

On supported systems:

```bash
sudo tcpdump -i any
```

This can observe traffic across multiple interfaces.

Useful for troubleshooting when the exact interface is unknown.

---

# Stop Capture

Interactive capture can normally be stopped with:

```text
Ctrl + C
```

tcpdump then prints capture statistics.

---

# Disable Name Resolution

A very useful option:

```bash
-n
```

Conceptually:

```bash
sudo tcpdump -n -i eth0
```

This prevents IP addresses from being resolved to hostnames.

Benefits:

```text
Faster output
Less DNS noise
Clear raw addresses
```

---

# Numeric Ports

Using numeric output also avoids translating ports into service names.

A commonly used form is:

```bash
sudo tcpdump -nn -i eth0
```

This is often preferable during packet analysis.

---

# Basic Output

Conceptual output:

```text
10.0.0.5.54321 > 10.0.0.10.443
```

Meaning:

```text
Source:
10.0.0.5:54321

Destination:
10.0.0.10:443
```

---

# Capture Filters

tcpdump supports **BPF-style capture filters**.

Filters reduce captured/displayed traffic.

Instead of:

```text
Capture Everything
```

use:

```text
Capture Relevant Traffic
```

---

# Filter by Host

```bash
sudo tcpdump -nn host 10.0.0.10
```

This captures traffic involving the specified host.

---

# Source Host

```bash
sudo tcpdump -nn src host 10.0.0.10
```

Only packets originating from that address.

---

# Destination Host

```bash
sudo tcpdump -nn dst host 10.0.0.10
```

Only packets destined for that address.

---

# Filter by Port

Example:

```bash
sudo tcpdump -nn port 443
```

Captures traffic involving port 443.

---

# Destination Port

```bash
sudo tcpdump -nn dst port 53
```

Useful for observing traffic destined for DNS.

---

# Filter by Protocol

TCP:

```bash
sudo tcpdump -nn tcp
```

UDP:

```bash
sudo tcpdump -nn udp
```

ICMP:

```bash
sudo tcpdump -nn icmp
```

---

# DNS Traffic

DNS commonly uses:

```text
UDP 53
```

and can also use TCP.

A simple observation filter:

```bash
sudo tcpdump -nn port 53
```

Conceptually:

```text
Client
  |
DNS Query
  |
  v
DNS Server
```

---

# HTTP Traffic

For unencrypted HTTP:

```bash
sudo tcpdump -nn port 80
```

Because HTTP is normally plaintext, application-layer information may be visible depending on capture options.

---

# HTTPS Traffic

For:

```text
HTTPS / TLS
```

payload contents are normally encrypted.

tcpdump can still reveal metadata such as:

```text
Source IP
Destination IP
Ports
Packet sizes
Timing
TCP behavior
```

But not normally plaintext application data.

---

# Logical Operators

Filters can be combined.

## AND

Example concept:

```text
host 10.0.0.10
AND
port 443
```

Meaning:

```text
Traffic involving host
10.0.0.10 on port 443
```

---

# OR

Conceptually:

```text
port 80
OR
port 443
```

captures traffic involving either web port.

---

# NOT

Conceptually:

```text
not port 22
```

can remove SSH traffic from a capture.

This is useful when capturing over an SSH connection because your own management traffic can otherwise create noise.

---

# Save PCAP

One of tcpdump's most useful features:

```bash
sudo tcpdump -i eth0 -w capture.pcap
```

This writes packets to:

```text
capture.pcap
```

rather than displaying every decoded packet.

---

# PCAP

**PCAP = Packet Capture**

A `.pcap` file can later be analyzed with:

```text
tcpdump
Wireshark
Other network-analysis tools
```

Conceptually:

```text
Network
 ↓
tcpdump
 ↓
capture.pcap
 ↓
Wireshark
```

---

# Read a PCAP

```bash
tcpdump -r capture.pcap
```

Numeric output:

```bash
tcpdump -nn -r capture.pcap
```

Apply a filter while reading:

```bash
tcpdump -nn -r capture.pcap port 53
```

This makes packet captures reusable.

---

# Tcpdump + Wireshark

A common remote-analysis workflow:

```text
Remote Linux Server
       ↓
tcpdump
       ↓
capture.pcap
       ↓
Authorized Analysis Workstation
       ↓
Wireshark
       ↓
Detailed Protocol Analysis
```

tcpdump is excellent for capture.

Wireshark is excellent for detailed graphical analysis.

---

# Packet Count

Sometimes only a limited number of packets are required.

Conceptually:

```text
Capture N packets
     ↓
Stop automatically
```

Check:

```bash
tcpdump --help
```

or the manual page for the installed version's packet-count option.

---

# Snapshot Length

Packet-capture tools can capture:

```text
Full packets
```

or only a limited number of bytes from each packet.

This affects:

```text
File size
Payload visibility
Performance
```

Modern tcpdump defaults are generally suitable for many tasks, but capture requirements should be considered.

---

# Verbosity

tcpdump provides different output verbosity levels.

More verbose output can expose additional protocol fields.

But:

```text
More Verbose
=
More Information
+
More Noise
```

Use the minimum level necessary.

---

# ASCII Payload

For plaintext protocols, tcpdump can display payload content in readable formats using appropriate options.

This can be useful for controlled lab protocols such as:

```text
HTTP
DNS
Other plaintext protocols
```

Be careful: packet payloads can contain sensitive information.

---

# TCP Flags

TCP uses flags such as:

```text
SYN
ACK
FIN
RST
PSH
```

Basic three-way handshake:

```text
Client                    Server

SYN --------------------->

    <---------------- SYN-ACK

ACK --------------------->
```

tcpdump makes this exchange visible.

---

# SYN

```text
SYN
```

is used when initiating a TCP connection.

---

# RST

```text
RST
```

resets a TCP connection.

Repeated resets can indicate:

```text
Closed ports
Application termination
Firewall behavior
Connection problems
```

Context matters.

---

# ICMP

ICMP supports network-control messages.

Example:

```text
Echo Request
Echo Reply
```

used by:

```text
ping
```

Capture:

```bash
sudo tcpdump -nn icmp
```

---

# ARP

On local IPv4 Ethernet networks, ARP maps:

```text
IPv4 Address
      ↓
MAC Address
```

Capture:

```bash
sudo tcpdump -nn arp
```

Conceptually:

```text
Who has 192.168.1.1?
        ↓
ARP Request
        ↓
192.168.1.1 is at aa:bb:cc...
```

---

# Troubleshooting Example

Suppose an application cannot connect to a database.

Capture:

```text
Application Host
      ↓
tcpdump
      ↓
Database Traffic
```

Then determine:

```text
Are packets leaving?

Does server respond?

Is TCP handshake completed?

Is connection reset?

Is DNS resolving correctly?
```

tcpdump helps answer network-layer questions quickly.

---

# Security Monitoring

Packet captures can help investigate:

```text
Unexpected outbound connections
DNS activity
Port scanning
Command-and-control indicators
Lateral movement
Unusual protocols
```

Packet data should be correlated with endpoint and application logs.

---

# Sensitive Information

PCAP files may contain:

```text
Credentials
Session tokens
Cookies
Emails
DNS queries
Internal IPs
Application data
```

especially when plaintext protocols are involved.

Therefore:

```text
PCAP = Potentially Sensitive Evidence
```

Protect capture files appropriately.

---

# Capture Size

Long captures can become very large.

Conceptually:

```text
Traffic Rate
    ×
Capture Duration
    =
PCAP Size
```

Use:

```text
Filters
Time limits
Packet limits
Capture rotation
```

where appropriate.

---

# Capture Filters vs Wireshark Display Filters

Important distinction:

```text
tcpdump / BPF capture filter
```

determines which packets are captured or selected.

Wireshark also has:

```text
Display Filters
```

for analyzing already captured traffic.

The syntaxes are different.

---

# Tcpdump vs Wireshark

```text
Tcpdump
 |
CLI
 |
Lightweight
 |
Remote/server friendly
```

versus:

```text
Wireshark
 |
GUI
 |
Deep protocol analysis
 |
Visual filtering
```

---

# Basic Cheat Sheet

Interfaces:

```bash
tcpdump -D
```

Capture:

```bash
sudo tcpdump -nn -i eth0
```

Host:

```bash
sudo tcpdump -nn host 10.0.0.10
```

Port:

```bash
sudo tcpdump -nn port 443
```

DNS:

```bash
sudo tcpdump -nn port 53
```

ICMP:

```bash
sudo tcpdump -nn icmp
```

Save:

```bash
sudo tcpdump -i eth0 -w capture.pcap
```

Read:

```bash
tcpdump -nn -r capture.pcap
```

---

# Analysis Workflow

```text
1. Identify interface

2. Define investigation question

3. Create narrow capture filter

4. Start capture

5. Reproduce event

6. Stop capture

7. Inspect packets

8. Save PCAP if required

9. Analyze deeply in Wireshark

10. Protect/delete evidence according to policy
```

---

# Key Takeaway

Tcpdump answers:

```text
"What packets are actually
crossing this network interface?"
```

Its core workflow is:

```text
INTERFACE
    ↓
FILTER
    ↓
CAPTURE
    ↓
PACKETS
    ↓
PCAP
    ↓
ANALYSIS
```

The most important skill is not memorizing every filter.

Start with a question:

```text
Which host?
Which protocol?
Which port?
Which direction?
```

and capture only the traffic required to answer it.
