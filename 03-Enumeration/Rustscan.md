# ⚡ RustScan

> RustScan is a fast port-discovery tool written in Rust. Its main strength is quickly identifying open ports and then handing those ports to tools such as Nmap for deeper service enumeration.

---

## Overview

Nmap is extremely capable, but scanning all TCP ports can take time depending on the target and network conditions.

RustScan focuses primarily on **fast port discovery**.

A useful workflow is:

```text
Target
   ↓
RustScan
   ↓
Open Ports
   ↓
Nmap
   ↓
Service Enumeration
```

RustScan does not replace Nmap.

It complements it.

---

## Why Use RustScan?

RustScan is useful for:

- Fast TCP port discovery
- CTFs
- Lab environments
- Quickly identifying non-standard ports
- Feeding discovered ports into Nmap

Example:

```text
RustScan finds:

22
80
8080
9001

↓

Nmap investigates those ports
```

---

## Installation

Installation methods can change between releases.

Check the official RustScan repository for the current recommended installation method.

Verify installation:

```bash
rustscan --version
```

Help:

```bash
rustscan --help
```

---

## Basic Scan

Authorized/lab example:

```bash
rustscan -a TARGET
```

Example:

```bash
rustscan -a 10.10.10.10
```

Potential output:

```text
Open 10.10.10.10:22
Open 10.10.10.10:80
Open 10.10.10.10:445
```

---

## Hostname

You can also use an authorized hostname:

```bash
rustscan -a target.example.test
```

---

## Multiple Hosts

Depending on the installed RustScan version, multiple addresses or ranges may be supported.

Always verify current syntax with:

```bash
rustscan --help
```

before large scans.

---

## Port Range

RustScan can be configured to scan selected ports/ranges rather than relying on defaults.

The exact option syntax may change between versions, so use the current CLI help.

The important concept is:

```text
Known Scope
   ↓
Choose Ports
   ↓
Discover Open Ports
```

---

## Batch Size

RustScan's speed comes partly from scanning many ports concurrently.

Aggressive settings can:

- Generate substantial traffic
- Trigger rate limiting
- Cause inaccurate results on unstable networks
- Violate engagement restrictions

Fast does not automatically mean better.

---

## RustScan + Nmap

This is the main reason RustScan is useful.

Conceptually:

```text
RustScan
    ↓
22,80,445
    ↓
Nmap
    ↓
Versions + Scripts
```

After discovery, you might manually run:

```bash
nmap -sC -sV -p 22,80,445 TARGET
```

This separates:

```text
Fast Discovery
```

from:

```text
Detailed Enumeration
```

---

## Why Not Use Only RustScan?

Port discovery alone gives you:

```text
22
80
445
```

But you still need to determine:

```text
22 → Which SSH implementation/version?

80 → Which web application/server?

445 → Which SMB environment?
```

That's where Nmap and manual enumeration become useful.

---

## Example Workflow

### Stage 1

```bash
rustscan -a TARGET
```

Suppose:

```text
22
80
445
```

are discovered.

### Stage 2

```bash
nmap -sC -sV -p 22,80,445 TARGET
```

### Stage 3

Move into service-specific enumeration:

```text
22  → SSH.md
80  → Web Pentesting
445 → SMB.md
```

---

## Non-Standard Ports

RustScan can help identify services running outside their traditional ports.

Example:

```text
2222 → SSH
8080 → HTTP
8443 → HTTPS
```

This reinforces an important rule:

> **Never assume service solely from port number.**

---

## RustScan vs Nmap

| RustScan | Nmap |
|---|---|
| Optimized for fast port discovery | Full network mapper |
| Very fast | More feature-rich |
| Finds open ports | Detects services/versions |
| Simple workflow | NSE scripting |
| Often used before Nmap | Primary enumeration tool |

Use them together rather than treating them as competitors.

---

## When to Use RustScan

Good situations:

- HTB
- THM
- VulnHub
- Your lab
- Authorized internal testing where fast discovery is appropriate

Less appropriate:

- Highly fragile networks
- Strictly rate-limited engagements
- Situations where traffic volume must be minimized

---

## Red Team Perspective

Bad workflow:

```text
RustScan
   ↓
Ports
   ↓
Search Exploit
```

Better:

```text
RustScan
   ↓
Ports
   ↓
Nmap
   ↓
Manual Enumeration
   ↓
Research
```

RustScan accelerates discovery.

It does not replace methodology.

---

## Common Beginner Mistakes

- Treating RustScan as a replacement for Nmap.
- Using excessive concurrency without considering the environment.
- Assuming default port mappings are always correct.
- Not manually enumerating discovered services.
- Scanning outside authorized scope.

---

## Interview Questions

### What is RustScan?

A fast port-discovery scanner written in Rust.

### RustScan vs Nmap?

RustScan emphasizes rapid port discovery; Nmap provides deeper service, OS, and script-based enumeration.

### Why combine them?

RustScan can quickly identify ports, while Nmap can investigate those ports in detail.

---

## Quick Revision

```text
RustScan
   ↓
Fast TCP Port Discovery
   ↓
Nmap
   ↓
Service Enumeration

Basic:
rustscan -a TARGET

Then:
nmap -sC -sV -p PORTS TARGET
```

---

## Practice

- Hack The Box
- TryHackMe
- VulnHub
- Local lab networks

---

## References

- RustScan Official GitHub Repository
- Nmap Documentation
- Hack The Box Academy
