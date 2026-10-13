
# Labs — Practical Cybersecurity Environments

> A collection of intentionally vulnerable applications and controlled virtual environments for hands-on cybersecurity education.

## Module Overview

This module covers practical lab environments used to develop skills in:

```text
Reconnaissance
Network Enumeration
Web Application Security
Vulnerability Research
Privilege Escalation
API Security
Docker
Virtualization
```

All exercises must remain inside authorized, isolated environments.

---

## Files

```text
14-Labs/
├── DVWA.md
├── Docker-Labs.md
├── Metasploitable2.md
├── OWASP-Juice-Shop.md
├── README.md
└── VulnHub.md
```

---

## Lab Platforms

| Lab | Focus |
|---|---|
| DVWA | Fundamental web vulnerabilities |
| Docker Labs | Reproducible container environments |
| Metasploitable 2 | Network/service vulnerabilities |
| OWASP Juice Shop | Modern web/API security |
| VulnHub | Vulnerable virtual machines |

---

## 1. Lab Objectives

A well-designed lab should help answer:

```text
What is the vulnerability?

Why does it occur?

How is it identified?

What security boundary fails?

How should it be fixed?
```

---

## 2. Lab Architecture

Recommended:

```text
Host Computer
      |
      +-- Attacker VM
      |
      +-- Isolated Lab Network
               |
               +-- Vulnerable VM
               +-- Vulnerable Web App
```

Use host-only networks or equivalent isolation.

---

## 3. Docker-Based Labs

Typical architecture:

```text
Docker Engine
      |
      +-- DVWA
      +-- Juice Shop
      +-- Other Test Services
```

Advantages:

```text
Fast Deployment
Reproducibility
Easy Reset
Low Resource Usage
```

---

## 4. VM-Based Labs

Typical architecture:

```text
Virtualization Platform
      |
      +-- Security Testing VM
      +-- Metasploitable
      +-- VulnHub VM
```

Advantages:

```text
Full Operating System
Service Enumeration
OS-Level Privilege Analysis
Realistic Network Behavior
```

---

## 5. Isolation Requirements

Never expose intentionally vulnerable labs directly to public networks.

Recommended controls:

```text
Host-Only Networking
Localhost Port Binding
No Unnecessary Port Forwarding
Restricted VM Access
No Production Credentials
```

---

## 6. Basic Lab Workflow

```text
Select Lab
    ↓
Deploy
    ↓
Verify Connectivity
    ↓
Create Snapshot / Reset Point
    ↓
Enumerate
    ↓
Test
    ↓
Document
    ↓
Restore
```

---

## 7. Web Lab Workflow

```text
Application
    ↓
Browser
    ↓
Burp / Caido
    ↓
HTTP Analysis
    ↓
Vulnerability Validation
    ↓
Root Cause
```

---

## 8. Network Lab Workflow

```text
Target VM
    ↓
Host Discovery
    ↓
Port Scanning
    ↓
Service Enumeration
    ↓
Vulnerability Research
    ↓
Controlled Validation
```

---

## 9. Recommended Tools

```text
Nmap
Burp Suite
Caido
Wireshark
tcpdump
FFUF
Gobuster
Feroxbuster
Nuclei
```

Use tools according to the lab objective.

---

## 10. Documentation

Record:

```text
Lab Name
Environment
Target IP
Open Ports
Vulnerability
Root Cause
Evidence
Mitigation
Lessons Learned
```

---

## 11. Lab Reset

Docker:

```bash
docker compose down
```

Reset persistent volumes when intended:

```bash
docker compose down -v
```

VMs:

```text
Restore Snapshot
```

Resetting helps keep experiments reproducible.

---

## 12. Learning Progression

Recommended:

```text
Docker Fundamentals
       ↓
DVWA
       ↓
OWASP Juice Shop
       ↓
Metasploitable 2
       ↓
VulnHub Machines
       ↓
Advanced Multi-System Labs
```

---

## 13. Lab Safety Checklist

```text
[ ] Authorized environment
[ ] Isolated networking
[ ] Trusted image/source
[ ] No real credentials
[ ] No production data
[ ] Snapshot available
[ ] Logging enabled
[ ] No public exposure
[ ] Reset procedure documented
```

---

## Key Takeaway

Practical labs connect:

```text
THEORY
   ↓
CONTROLLED EXPERIMENT
   ↓
OBSERVATION
   ↓
ROOT CAUSE
   ↓
REMEDIATION
```

The goal is not simply exploiting a vulnerable application.

It is understanding the complete vulnerability lifecycle.
