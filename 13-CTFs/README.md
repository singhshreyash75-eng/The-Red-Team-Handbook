
# CTFs — Capture The Flag

> A structured guide to practical cybersecurity training platforms, CTF methodology, lab documentation, and offensive-security skill development.

## Module Overview

**CTF = Capture The Flag**

CTFs are controlled cybersecurity challenges designed to develop technical problem-solving skills.

They provide hands-on experience with:

```text
Reconnaissance
Enumeration
Web Security
Networking
Cryptography
Reverse Engineering
Binary Exploitation
Privilege Escalation
Active Directory
Digital Forensics
```

---

## Files

```text
13-CTFs/
├── HackTheBox.md
├── PicoCTF.md
├── PortSwigger.md
├── README.md
└── TryHackMe.md
```

---

## 1. CTF Platforms

| Platform | Main Focus |
|---|---|
| Hack The Box | Realistic machines, penetration testing, AD |
| picoCTF | Foundational challenges, crypto, reversing |
| PortSwigger Academy | Web application security |
| TryHackMe | Guided cybersecurity labs and learning paths |

Each platform serves a different learning purpose.

---

## 2. CTF Categories

Common categories:

```text
Web
Cryptography
Forensics
Reverse Engineering
Binary Exploitation
OSINT
Networking
Miscellaneous
```

---

## 3. Jeopardy-Style CTFs

Participants solve independent challenges.

```text
Challenge A → Flag
Challenge B → Flag
Challenge C → Flag
```

Challenges are commonly grouped by category and difficulty.

---

## 4. Attack-Defense CTFs

Teams may need to:

```text
Defend their services
Identify vulnerabilities
Patch weaknesses
Maintain availability
Attack opposing lab services
```

These are conducted under competition-specific rules.

---

## 5. Boot-to-Root Labs

A typical machine challenge:

```text
Target
   ↓
Enumeration
   ↓
Initial Access
   ↓
Privilege Escalation
   ↓
Root / Administrator Proof
```

Hack The Box and TryHackMe frequently use this learning style.

---

## 6. Web Security Labs

A typical web lab:

```text
Application
     ↓
HTTP Analysis
     ↓
Identify Input
     ↓
Understand Trust Boundary
     ↓
Validate Vulnerability
```

PortSwigger Academy specializes in this area.

---

## 7. Recommended Tool Categories

Reconnaissance:

```text
Nmap
Subfinder
Amass
httpx
```

Web testing:

```text
Burp Suite
Caido
FFUF
Feroxbuster
```

Network analysis:

```text
Wireshark
tcpdump
```

Reverse engineering:

```text
Ghidra
GDB
objdump
```

Scripting:

```text
Python
Bash
PowerShell
Go
```

---

## 8. General CTF Methodology

```text
READ
  ↓
ENUMERATE
  ↓
ANALYZE
  ↓
HYPOTHESIZE
  ↓
TEST
  ↓
VERIFY
  ↓
DOCUMENT
```

Do not jump immediately to exploitation.

---

## 9. Reconnaissance

Collect:

```text
Target IP
Open ports
Services
Versions
Web technologies
DNS information
```

Example lab scan:

```bash
nmap -sV -sC 10.10.10.10
```

---

## 10. Enumeration

Investigate each exposed service.

```text
HTTP → Application mapping
SMB → Shares and permissions
SSH → Authentication configuration
DNS → Records
LDAP → Directory information
```

Enumeration is often the most important phase of a machine challenge.

---

## 11. Vulnerability Research

```text
Service
   ↓
Version
   ↓
CVE Research
   ↓
Prerequisites
   ↓
Lab Validation
```

Useful sources:

```text
NVD
CVE Program
Exploit-DB
Vendor advisories
```

---

## 12. Privilege Escalation

Linux:

```text
sudo
SUID
Capabilities
Cron
Services
Permissions
```

Windows:

```text
Privileges
Services
Scheduled Tasks
ACLs
Groups
Credentials
```

---

## 13. Active Directory

Important topics:

```text
Kerberos
LDAP
SPNs
ACLs
Group Membership
Trusts
BloodHound
```

Understand identity relationships rather than relying solely on tools.

---

## 14. Cryptography

Core topics:

```text
Encoding
Hashing
Symmetric Encryption
Asymmetric Encryption
Modular Arithmetic
Classical Ciphers
```

Remember:

```text
Encoding != Encryption
Hashing != Encryption
```

---

## 15. Reverse Engineering

Workflow:

```text
Identify Binary
      ↓
Inspect Strings
      ↓
Analyze Functions
      ↓
Debug
      ↓
Understand Program Logic
```

---

## 16. Forensics

Common artifacts:

```text
PCAP
Disk Images
Memory Dumps
Logs
Documents
Images
Archives
```

Useful tools:

```text
Wireshark
ExifTool
binwalk
strings
file
```

---

## 17. CTF Writeups

A good writeup contains:

```text
Challenge Overview
Initial Observations
Enumeration
Vulnerability
Technical Explanation
Evidence
Root Cause
Mitigation
Lessons Learned
```

Do not publish flags or spoilers for active competitions where rules prohibit them.

---

## 18. Writeup Template

```text
# Challenge Name

## Platform

## Category

## Difficulty

## Objective

## Initial Enumeration

## Vulnerability Analysis

## Solution Methodology

## Evidence

## Root Cause

## Mitigation

## Lessons Learned
```

---

## 19. Learning Strategy

Recommended progression:

```text
picoCTF
   ↓
TryHackMe Guided Labs
   ↓
PortSwigger Web Labs
   ↓
Hack The Box Easy Machines
   ↓
Medium Machines
   ↓
Active Directory Labs
   ↓
Advanced Challenges
```

This is a suggested progression, not a strict prerequisite chain.

---

## 20. Avoid Walkthrough Dependency

Better approach:

```text
Attempt Independently
        ↓
Record What Is Known
        ↓
Identify Missing Concept
        ↓
Use Small Hint
        ↓
Continue
```

After completion, explain the solution without consulting the walkthrough.

---

## 21. Skill Tracking

Track:

```text
Challenge
Platform
Category
Difficulty
Completion Date
Tools
New Concepts
Mistakes
```

A progress log makes it easier to identify weak areas.

---

## 22. CTF vs Real Penetration Testing

CTFs:

```text
Designed vulnerabilities
Known objectives
Controlled environments
Flags
```

Real assessments:

```text
Business constraints
Defined authorization
Uncertain vulnerabilities
Production impact
Reporting requirements
```

CTF skills transfer to professional security work, but the methodology must account for real-world constraints.

---

## Key Takeaway

CTFs develop:

```text
TECHNICAL KNOWLEDGE
       +
PROBLEM SOLVING
       +
PERSISTENCE
       +
DOCUMENTATION
```

The goal is not to collect flags.

It is to build repeatable security reasoning and understand why a vulnerability or technique works.
