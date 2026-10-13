
# Cybersecurity Cheat Sheets

> A curated collection of practical references for Linux, networking, web security, Active Directory, penetration testing, and vulnerability research.

## 1. Overview

Cheat sheets provide fast access to:

```text
Commands
Syntax
Payload references
Protocol details
Testing methodologies
Tool options
```

They are most useful when the underlying concepts are already understood.

---

## 2. HackTricks

**Website:** https://book.hacktricks.wiki/

**Focus:**
- Linux privilege escalation
- Windows privilege escalation
- Active Directory
- Web security
- Cloud security
- Enumeration
- Security tools

**Why use it:**

One of the most comprehensive community-maintained references for penetration-testing techniques.

---

## 3. PayloadsAllTheThings

**Repository:** https://github.com/swisskyrepo/PayloadsAllTheThings

**Focus:**
- SQL Injection
- XSS
- SSRF
- SSTI
- Command Injection
- File Upload
- Authentication
- API security

**Why use it:**

Provides vulnerability-specific payload references and testing methodologies.

Use only relevant, understood payloads in authorized environments.

---

## 4. GTFOBins

**Website:** https://gtfobins.github.io/

**Focus:**
- Unix binaries
- SUID
- sudo
- Capabilities
- Restricted environments

**Why use it:**

Documents security-relevant behaviors of legitimate Unix executables.

Useful for privilege reviews and hardening.

---

## 5. LOLBAS

**Website:** https://lolbas-project.github.io/

**Focus:**
- Windows signed binaries
- Legitimate administrative utilities
- Execution behavior
- Defensive detection
- Living-off-the-land techniques

**Why use it:**

Helps researchers and defenders understand how trusted Windows utilities can be abused.

---

## 6. SecLists

**Repository:** https://github.com/danielmiessler/SecLists

**Focus:**
- Web discovery
- DNS
- Password auditing
- Usernames
- Fuzzing
- Test data

**Why use it:**

Provides organized wordlists for different security-testing objectives.

---

## 7. OWASP Cheat Sheet Series

**Website:** https://cheatsheetseries.owasp.org/

**Focus:**
- Authentication
- Authorization
- Session management
- Input validation
- SQL injection prevention
- XSS prevention
- Secure development

**Why use it:**

Excellent for understanding the defensive implementation of security controls.

---

## 8. Nmap Reference Guide

**Website:** https://nmap.org/book/man.html

**Focus:**
- Host discovery
- TCP scanning
- UDP scanning
- Service detection
- NSE scripts
- Output formats

**Why use it:**

Authoritative documentation for Nmap behavior and options.

---

## 9. PortSwigger Web Security Academy

**Website:** https://portswigger.net/web-security

**Focus:**
- Web vulnerability classes
- Testing methodology
- HTTP behavior
- Burp Suite
- Interactive labs

**Why use it:**

Combines theory with practical web-security exercises.

---

## 10. Linux Manual Pages

Local command reference:

```bash
man ssh
man find
man grep
man chmod
```

Search manual pages:

```bash
man -k network
```

**Why use it:**

The local manual usually matches the installed tool version.

---

## 11. Microsoft Learn

**Website:** https://learn.microsoft.com/

**Focus:**
- PowerShell
- Windows security
- Active Directory
- Microsoft Entra
- Azure
- Windows administration

**Why use it:**

Authoritative documentation for Microsoft technologies.

---

## 12. MITRE ATT&CK

**Website:** https://attack.mitre.org/

**Focus:**
- Adversary tactics
- Techniques
- Detection
- Mitigation
- Threat intelligence

**Why use it:**

Provides a structured framework for understanding adversary behavior.

---

## 13. MITRE ATLAS

**Website:** https://atlas.mitre.org/

**Focus:**
- Adversarial machine learning
- AI security
- ML attack techniques
- AI threat modeling

**Why use it:**

Useful for studying AI-specific security threats and attack taxonomies.

---

## 14. Internal Handbook Cheat Sheets

This repository also contains:

```text
12-CheatSheets/
├── Active-Directory.md
├── Burp.md
├── File-Transfer.md
├── Linux.md
├── Networking.md
├── Nmap.md
├── One-Liners.md
├── PrivEsc.md
├── README.md
├── Reverse-Shell.md
├── SQLi.md
├── Windows.md
└── XSS.md
```

These are intended for quick offline reference.

---

## 15. Recommended Usage

For a new security assessment:

```text
Identify Technology
       ↓
Read Official Documentation
       ↓
Consult Relevant Cheat Sheet
       ↓
Understand Technique
       ↓
Perform Authorized Testing
       ↓
Validate Result
```

---

## 16. Choosing a Reference

| Task | Reference |
|---|---|
| Linux privilege review | HackTricks, GTFOBins |
| Windows security | Microsoft Learn, LOLBAS |
| Web vulnerability research | PortSwigger, PayloadsAllTheThings |
| Secure coding | OWASP Cheat Sheet Series |
| Wordlists | SecLists |
| Nmap commands | Nmap Reference Guide |
| Adversary techniques | MITRE ATT&CK |
| AI security | MITRE ATLAS |

---

## Key Takeaway

Cheat sheets are most effective when used as:

```text
QUICK REFERENCE
      +
TECHNICAL CONTEXT
      +
PRACTICAL VALIDATION
```

Do not treat a copied command or payload as proof of a vulnerability.

Understand its purpose, preconditions, and potential impact.
