# 🔬 Enumeration

> Enumeration is the systematic process of interacting with discovered services to determine what they are, how they are configured, and what information they expose.

---

## Overview

Recon answers:

> **What exists?**

Enumeration asks:

> **What exactly is running there?**

Example:

```text
Recon

↓

10.10.10.10 is alive

↓

Enumeration

↓

22/tcp → OpenSSH
80/tcp → nginx
445/tcp → SMB
```

Now each service can be investigated individually.

---

## Recon vs Enumeration vs Exploitation

```text
Recon
   ↓
Discover Assets

Enumeration
   ↓
Understand Services

Vulnerability Analysis
   ↓
Identify Weaknesses

Exploitation
   ↓
Validate Impact
```

Do not immediately jump from:

```text
Port Open
```

to:

```text
Exploit
```

Enumeration comes first.

---

## Why Enumeration Matters

Enumeration can reveal:

- Services
- Versions
- Hostnames
- Domains
- Users
- Shares
- Authentication methods
- Software versions
- Application banners
- Network roles

This information determines the next stage of the assessment.

---

## Module Structure

```text
03-Enumeration/

├── README.md
├── Nmap.md
├── Rustscan.md
├── SMB.md
├── FTP.md
├── SSH.md
├── SMTP.md
├── DNS.md
├── SNMP.md
├── LDAP.md
└── RPC.md
```

---

## Basic Enumeration Workflow

```text
Target
  ↓
Host Discovery
  ↓
Port Scan
  ↓
Service Detection
  ↓
Service Enumeration
  ↓
Version Research
  ↓
Prioritize
```

---

## Port Scanning

First identify exposed ports.

Example authorized/lab scan:

```bash
nmap TARGET
```

More comprehensive TCP-port discovery:

```bash
nmap -p- TARGET
```

Then perform targeted service/version enumeration against discovered ports.

---

## Service Detection

Example:

```bash
nmap -sV TARGET
```

Potential output:

```text
22/tcp  open  ssh   OpenSSH
80/tcp  open  http  nginx
445/tcp open  smb
```

Now you know where deeper enumeration should focus.

---

## Service-Specific Enumeration

Examples:

### Port 21

```text
FTP
```

Investigate:

- Authentication
- Anonymous access
- Files

### Port 22

```text
SSH
```

Investigate:

- Version
- Authentication methods

### Port 53

```text
DNS
```

Investigate:

- Records
- Name servers
- Zone configuration

### Port 80 / 443

```text
HTTP(S)
```

Move into web enumeration.

### Port 445

```text
SMB
```

Investigate:

- Shares
- Domain/workgroup information
- Access permissions

### Port 389

```text
LDAP
```

Common in Active Directory environments.

---

## Common Ports

| Port | Service |
|---:|---|
| 21 | FTP |
| 22 | SSH |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 111 | RPCbind |
| 135 | MSRPC |
| 139 | NetBIOS |
| 143 | IMAP |
| 161 | SNMP |
| 389 | LDAP |
| 443 | HTTPS |
| 445 | SMB |
| 636 | LDAPS |
| 2049 | NFS |
| 3306 | MySQL |
| 3389 | RDP |
| 5432 | PostgreSQL |
| 5985 | WinRM |

Ports provide clues, but service detection matters more than assuming a service purely from its default port.

---

## Enumeration Mindset

For every open service, ask:

```text
What is it?

Which version?

What information is exposed?

Does it require authentication?

Is anonymous access supported?

What role does this service play?

Is its configuration unusual?

Are known vulnerabilities relevant?
```

---

## Banner Grabbing

Some services reveal identifying information when connected to.

For example, in an authorized lab:

```bash
nc TARGET PORT
```

A banner may disclose:

```text
Service
Version
Hostname
```

Not every service provides a useful banner.

---

## Version Research

After identifying a version:

```text
Product
+
Version
```

research:

- Vendor advisories
- NVD
- CISA KEV where relevant
- Release notes
- ExploitDB/Searchsploit as secondary references

A version match alone does **not** prove vulnerability.

Backported patches are common.

---

## Keep Structured Notes

Example:

```text
Host:
10.10.10.10

22/tcp:
OpenSSH

80/tcp:
nginx
Login page

445/tcp:
SMB
Shares discovered

Priority:
SMB + Web
```

---

## Red Team Perspective

Enumeration is often where good pentesters separate themselves from beginners.

Beginner:

```text
Nmap
 ↓
Search exploit
```

Better:

```text
Nmap
 ↓
Understand service
 ↓
Enumerate configuration
 ↓
Understand authentication
 ↓
Research version
 ↓
Form hypothesis
 ↓
Validate safely
```

---

## Common Beginner Mistakes

- Scanning only default ports.
- Ignoring UDP entirely.
- Assuming port number equals service.
- Treating version strings as proof of vulnerability.
- Running automated exploits before understanding the service.
- Failing to document findings.
- Ignoring authentication and access-control configuration.

---

## Quick Revision

```text
Recon = Find it

Enumeration = Understand it

Exploitation = Validate weakness

Workflow:

Ports
 ↓
Services
 ↓
Versions
 ↓
Configuration
 ↓
Users/Shares/Data
 ↓
Prioritize
```

---

## Practice

Recommended:

- Hack The Box
- HTB Academy
- TryHackMe
- Metasploitable
- VulnHub
- Your own lab network

---

## References

- Nmap Documentation
- NIST SP 800-115
- OWASP Web Security Testing Guide
- Hack The Box Academy
- TryHackMe
