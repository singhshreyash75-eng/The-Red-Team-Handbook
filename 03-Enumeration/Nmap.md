# 🗺️ Nmap

> Nmap (Network Mapper) is a network discovery and service-enumeration tool used to identify reachable hosts, exposed ports, services, versions, and selected configuration information.

---

## Overview

Nmap is one of the most important tools in penetration testing.

A typical workflow:

```text
Target
  ↓
Host Discovery
  ↓
Port Discovery
  ↓
Service Detection
  ↓
NSE Enumeration
  ↓
Manual Service Enumeration
```

Nmap should tell you **where to investigate next**, not replace investigation.

---

## Basic Scan

Authorized/lab example:

```bash
nmap TARGET
```

By default, Nmap performs a TCP scan against a commonly used set of ports.

Example output:

```text
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
443/tcp open  https
```

---

## Port States

Common states include:

### Open

```text
open
```

A service is accepting connections.

### Closed

```text
closed
```

The host is reachable, but nothing is listening on that port.

### Filtered

```text
filtered
```

Nmap cannot reliably determine the state, often because packet filtering interferes with probes.

---

## Scan Specific Port

```bash
nmap -p 443 TARGET
```

Multiple ports:

```bash
nmap -p 22,80,443 TARGET
```

Range:

```bash
nmap -p 1-1000 TARGET
```

---

## All TCP Ports

```bash
nmap -p- TARGET
```

This checks TCP ports:

```text
1–65535
```

A common assessment pattern is to perform port discovery first, then run more detailed probes only against identified ports.

---

## Service Version Detection

```bash
nmap -sV TARGET
```

Example:

```text
22/tcp open ssh  OpenSSH
80/tcp open http nginx
```

Version information can help guide further enumeration and vulnerability research.

---

## Default Scripts

```bash
nmap -sC TARGET
```

`-sC` runs Nmap's default NSE script set.

These scripts can collect additional information depending on the exposed services.

---

## Combined Enumeration

A common lab workflow after identifying ports:

```bash
nmap -sC -sV -p 22,80,445 TARGET
```

This is generally preferable to running expensive checks indiscriminately against every possible port.

---

## SYN Scan

```bash
sudo nmap -sS TARGET
```

A SYN scan performs TCP discovery using SYN packets rather than completing ordinary application connections for every probe.

Raw-packet capabilities typically require elevated privileges.

---

## TCP Connect Scan

```bash
nmap -sT TARGET
```

This uses the operating system's normal TCP `connect()` behavior.

It is useful when raw-packet privileges are unavailable.

---

## SYN Scan Concept

Normal TCP handshake:

```text
Client          Server
  │                │
  │ ---- SYN ----> │
  │ <--- SYN/ACK - │
  │ ---- ACK ----> │
```

SYN scanning uses the early handshake behavior to infer port state without proceeding like a normal application connection.

---

## UDP Scanning

UDP services can be important.

Examples:

```text
53  DNS
67  DHCP
123 NTP
161 SNMP
```

Authorized/lab scan:

```bash
sudo nmap -sU TARGET
```

UDP scanning can be considerably slower and harder to interpret than TCP scanning because many UDP services do not respond to arbitrary probes.

---

## Host Discovery

Basic host discovery:

```bash
nmap -sn 192.0.2.0/24
```

This performs host discovery without the normal port-scan phase.

Useful for authorized internal lab networks.

---

## Skip Host Discovery

Some environments block discovery probes while still exposing services.

```bash
nmap -Pn TARGET
```

This tells Nmap to treat the target as up and proceed with scanning.

It does **not** make scanning stealthy.

---

## OS Detection

```bash
sudo nmap -O TARGET
```

Nmap attempts to infer the operating system from network behavior.

Results are probabilistic and should not be treated as absolute truth.

---

## `-A`

```bash
sudo nmap -A TARGET
```

`-A` enables several advanced detection features.

It can generate substantially more traffic than a basic scan.

Use deliberately rather than as your default command.

---

## Output Formats

Normal output:

```bash
nmap TARGET -oN scan.txt
```

XML:

```bash
nmap TARGET -oX scan.xml
```

All major formats:

```bash
nmap TARGET -oA scans/target
```

`-oA` is useful because structured output can later be parsed by other tools.

---

## Nmap Scripting Engine (NSE)

NSE extends Nmap using scripts.

Scripts can perform tasks such as:

- Service enumeration
- Protocol queries
- Configuration checks
- Selected vulnerability checks

List installed scripts on common Linux installations:

```bash
ls /usr/share/nmap/scripts/
```

---

## Running a Specific Script

Example against an authorized HTTP service:

```bash
nmap --script http-title -p 80 TARGET
```

For SMB information:

```bash
nmap --script smb-os-discovery -p 445 TARGET
```

Exact script availability depends on the installed Nmap version.

---

## Script Categories

NSE scripts are organized into categories such as:

```text
auth
default
discovery
safe
version
vuln
```

Read a script's documentation before running it.

Not every NSE script is appropriate for every engagement.

---

## Timing Templates

Nmap supports:

```text
-T0
-T1
-T2
-T3
-T4
-T5
```

Higher values generally make scanning more aggressive/faster.

`-T4` is common in controlled labs and suitable networks, but there is no universally correct timing profile.

Consider:

- Network reliability
- Scope
- Rate restrictions
- IDS/IPS impact
- Service stability

---

## Example Lab Workflow

### Stage 1 — Discover TCP Ports

```bash
nmap -p- TARGET -oN all-ports.txt
```

### Stage 2 — Enumerate Discovered Ports

Suppose you found:

```text
22
80
445
```

Then:

```bash
nmap -sC -sV -p 22,80,445 TARGET -oN services.txt
```

### Stage 3 — Manual Enumeration

```text
22  → SSH.md
80  → Web Pentesting
445 → SMB.md
```

This is the important part.

**Nmap identifies the doors. Service enumeration investigates what is behind them.**

---

## Don't Blindly Trust Service Names

Suppose Nmap shows:

```text
8080/tcp open http
```

That's reasonable.

But a custom application could run:

```text
SSH on 2222
HTTP on 9000
```

Ports are conventions, not guarantees.

Service detection and manual validation matter.

---

## Version Research

Suppose you identify:

```text
OpenSSH X.Y
```

Don't immediately conclude:

```text
Version → CVE → Vulnerable
```

Instead:

```text
Version
   ↓
Operating System / Package Context
   ↓
Vendor Advisory
   ↓
Patch / Backport Status
   ↓
Configuration
   ↓
Actual Exposure
```

Linux distributions frequently backport security fixes without changing the upstream-looking version in the way beginners expect.

---

## Firewall Interpretation

Example:

```text
22/tcp open
80/tcp filtered
443/tcp closed
```

Interpretation:

```text
22 → Service accepting connections
80 → Filtering prevents clear determination
443 → Host reachable, no listener
```

Understanding states is more important than merely listing ports.

---

## Nmap + Recon Workflow

```text
Recon
  ↓
IP / Host
  ↓
Nmap
  ↓
Ports
  ↓
Services
  ↓
Service-Specific Enumeration
```

Examples:

```text
445 → SMB
389 → LDAP
161/UDP → SNMP
53 → DNS
```

Each gets its own chapter in this module.

---

## Notes Template

```text
Host:
10.10.10.10

TCP:

22
OpenSSH

80
nginx
Web application

445
SMB

UDP:
Not yet tested

Priority:
Web + SMB
```

---

## Red Team Perspective

Bad workflow:

```text
nmap -A TARGET
      ↓
Copy output
      ↓
Search random exploits
```

Better workflow:

```text
Port Discovery
      ↓
Targeted Service Detection
      ↓
Understand Services
      ↓
Manual Enumeration
      ↓
Research
      ↓
Form Hypothesis
```

Nmap is a **decision-making tool**, not an exploitation strategy.

---

## Common Beginner Mistakes

- Scanning only default TCP ports.
- Forgetting UDP completely.
- Using `-A` for everything.
- Treating service fingerprints as perfect.
- Assuming a version string proves vulnerability.
- Ignoring `filtered` ports.
- Running NSE scripts without understanding them.
- Failing to save scan output.
- Running large scans outside authorized scope.

---

## Interview Questions

### What is Nmap?

A network discovery and service-enumeration tool.

### `-sS` vs `-sT`?

`-sS` uses SYN-based probing; `-sT` uses the OS TCP connect mechanism.

### What does `-sV` do?

Attempts service/version detection.

### What does `-sC` do?

Runs the default NSE script set.

### What does `-Pn` do?

Skips Nmap's normal host-discovery phase and treats the target as up.

### Open vs closed vs filtered?

Open has a listening service; closed is reachable without a listener; filtered cannot be reliably determined because of filtering.

### Why scan UDP?

Important protocols such as DNS and SNMP may use UDP.

---

## Quick Revision

```text
Basic:
nmap TARGET

All TCP Ports:
nmap -p- TARGET

Versions:
nmap -sV TARGET

Default Scripts:
nmap -sC TARGET

Targeted:
nmap -sC -sV -p PORTS TARGET

UDP:
sudo nmap -sU TARGET

Host Discovery:
nmap -sn NETWORK

Skip Discovery:
nmap -Pn TARGET

Save:
-oN
-oX
-oA
```

---

## Practice

Excellent environments:

- Hack The Box
- HTB Academy
- TryHackMe
- Metasploitable
- VulnHub
- Your own lab network

---

## References

- Nmap Reference Guide
- Nmap Network Scanning — Gordon Lyon
- Nmap NSE Documentation
- Hack The Box Academy
- TryHackMe
