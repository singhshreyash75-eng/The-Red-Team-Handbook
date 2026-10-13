
# Nmap — Cheat Sheet

> Practical Nmap commands for authorized host discovery, port scanning, service enumeration, and network-security assessments.

## 1. What is Nmap?

**Nmap = Network Mapper**

Nmap is a network discovery and security-auditing tool.

Common capabilities:

```text
Host Discovery
Port Scanning
Service Detection
OS Fingerprinting
NSE Scripts
Network Inventory
```

---

## 2. Basic Scan

```bash
nmap 192.0.2.10
```

By default, Nmap scans a selection of common TCP ports.

Use only authorized hosts.

---

## 3. Scan Multiple Hosts

```bash
nmap 192.0.2.10 192.0.2.11
```

Authorized subnet:

```bash
nmap 192.0.2.0/28
```

Input list:

```bash
nmap -iL targets.txt
```

---

## 4. Host Discovery

Ping scan:

```bash
nmap -sn 192.0.2.0/28
```

This performs host discovery without a normal port scan.

---

## 5. TCP SYN Scan

```bash
sudo nmap -sS 192.0.2.10
```

SYN scanning examines TCP handshake responses.

Common interpretation:

```text
SYN-ACK → Open
RST     → Closed
No response / certain ICMP errors → Filtered
```

Results depend on network conditions.

---

## 6. TCP Connect Scan

```bash
nmap -sT 192.0.2.10
```

Uses operating-system TCP connection facilities.

Useful when raw-packet privileges are unavailable.

---

## 7. UDP Scan

```bash
sudo nmap -sU 192.0.2.10
```

UDP scanning can be slower and more ambiguous than TCP scanning.

Example:

```text
open|filtered
```

means Nmap cannot distinguish the two states from available evidence.

---

## 8. Specific Ports

```bash
nmap -p 22,80,443 192.0.2.10
```

Port range:

```bash
nmap -p 1-1000 192.0.2.10
```

All TCP ports:

```bash
nmap -p- 192.0.2.10
```

---

## 9. Top Ports

```bash
nmap --top-ports 100 192.0.2.10
```

Useful for quicker initial enumeration.

---

## 10. Service Detection

```bash
nmap -sV 192.0.2.10
```

Example conceptual output:

```text
22/tcp   open   ssh
80/tcp   open   http
443/tcp  open   https
```

Version information is a fingerprint, not proof of vulnerability.

---

## 11. OS Detection

```bash
sudo nmap -O 192.0.2.10
```

OS detection uses network fingerprints.

Accuracy depends on:

```text
Network conditions
Available ports
Firewalls
Target behavior
```

---

## 12. Default NSE Scripts

```bash
nmap -sC 192.0.2.10
```

Equivalent to running Nmap's default script selection.

Default scripts can interact with services; confirm scope and acceptable impact.

---

## 13. Combined Enumeration

```bash
sudo nmap -sS -sV -sC -O 192.0.2.10
```

Provides:

```text
Port States
Service Information
Default NSE Results
OS Fingerprint
```

---

## 14. Nmap Scripting Engine

**NSE = Nmap Scripting Engine**

Scripts support tasks such as:

```text
Discovery
Service Enumeration
Configuration Checks
Protocol Information
Security Auditing
```

List installed scripts:

```bash
ls /usr/share/nmap/scripts/
```

Location varies by installation.

---

## 15. NSE Script Categories

Common categories include:

```text
default
safe
discovery
auth
version
vuln
intrusive
brute
dos
exploit
```

Important:

Some categories can be disruptive.

Do not run intrusive, brute-force, DoS, or exploit scripts against production systems without explicit authorization.

---

## 16. Safe Script Example

Retrieve HTTP title:

```bash
nmap -p 80,443 --script http-title 192.0.2.10
```

Retrieve HTTP headers:

```bash
nmap -p 80,443 --script http-headers 192.0.2.10
```

---

## 17. SSH Enumeration

```bash
nmap -p 22 -sV 192.0.2.10
```

Inspect SSH algorithms:

```bash
nmap -p 22 --script ssh2-enum-algos 192.0.2.10
```

---

## 18. SMB Enumeration

```bash
nmap -p 445 -sV 192.0.2.10
```

Protocol information:

```bash
nmap -p 445 --script smb-protocols 192.0.2.10
```

Useful for identifying legacy SMB protocol support.

---

## 19. DNS Enumeration

```bash
nmap -sU -p 53 192.0.2.10
```

TCP DNS:

```bash
nmap -sT -p 53 192.0.2.10
```

---

## 20. Scan Timing

Nmap timing templates:

```text
-T0 Paranoid
-T1 Sneaky
-T2 Polite
-T3 Normal
-T4 Aggressive
-T5 Insane
```

Timing affects speed and network load.

For authorized production assessments, prioritize stability and explicit rate controls over speed.

---

## 21. Skip Host Discovery

```bash
nmap -Pn 192.0.2.10
```

Useful when ICMP/host discovery is blocked.

Important:

```text
-Pn
```

does not prove the host is online.

It tells Nmap to proceed as though it is.

---

## 22. DNS Resolution

Skip reverse DNS:

```bash
nmap -n 192.0.2.10
```

This can reduce DNS-related overhead.

---

## 23. Output Formats

Normal:

```bash
nmap -oN scan.txt 192.0.2.10
```

XML:

```bash
nmap -oX scan.xml 192.0.2.10
```

Grepable:

```bash
nmap -oG scan.gnmap 192.0.2.10
```

All major output formats:

```bash
nmap -oA scan 192.0.2.10
```

XML is useful for structured processing.

---

## 24. Port States

| State | Meaning |
|---|---|
| open | Application accepting connections |
| closed | Reachable but no service accepting |
| filtered | Filtering prevents determination |
| unfiltered | Accessible but open/closed unknown |
| open\|filtered | Either open or filtered |
| closed\|filtered | Either closed or filtered |

---

## 25. Nmap vs Netcat

```text
Nmap
  |
Broad host/port/service discovery
```

```text
Netcat
  |
Individual connection testing
and simple TCP/UDP interactions
```

They complement each other.

---

## 26. Nmap + Vulnerability Research

Correct workflow:

```text
Nmap
  ↓
Service / Version
  ↓
Vendor Advisory
  ↓
CVE / NVD
  ↓
Configuration Verification
  ↓
Manual Validation
```

Never assume an old-looking banner automatically proves vulnerability.

---

## 27. Scan Methodology

```text
1. Confirm authorized scope

2. Host discovery

3. Common TCP ports

4. Additional scoped ports

5. Service/version detection

6. Relevant safe NSE scripts

7. Record output

8. Research findings

9. Validate manually
```

---

## 28. Quick Command Reference

```bash
# Basic
nmap 192.0.2.10

# Host discovery
nmap -sn 192.0.2.0/28

# SYN
sudo nmap -sS 192.0.2.10

# TCP connect
nmap -sT 192.0.2.10

# UDP
sudo nmap -sU 192.0.2.10

# Service versions
nmap -sV 192.0.2.10

# Default scripts
nmap -sC 192.0.2.10

# OS detection
sudo nmap -O 192.0.2.10

# Specific ports
nmap -p 22,80,443 192.0.2.10

# All TCP ports
nmap -p- 192.0.2.10

# Save results
nmap -oA scan 192.0.2.10
```

---

## Key Takeaway

Nmap's core workflow:

```text
DISCOVER HOST
      ↓
IDENTIFY PORTS
      ↓
IDENTIFY SERVICES
      ↓
ENUMERATE CONFIGURATION
      ↓
RESEARCH
      ↓
VALIDATE
```

**An open port is an observation, not a vulnerability.**
