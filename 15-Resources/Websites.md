
# Cybersecurity Websites

> Essential websites for vulnerability intelligence, security research, technical documentation, hands-on learning, and offensive-security development.

## 1. Overview

Cybersecurity websites serve different purposes:

```text
Vulnerability Intelligence
Security Research
Technical Documentation
Practical Labs
Threat Intelligence
Security News
AI Security
```

Choose sources according to the task.

---

## 2. Vulnerability Databases

### CVE Program

https://www.cve.org/

**Purpose:** Standardized vulnerability identifiers.

Example:

```text
CVE-YYYY-NNNN
```

A CVE identifier identifies a publicly tracked vulnerability; it does not automatically establish exploitability.

### National Vulnerability Database

https://nvd.nist.gov/

**Purpose:**

- Vulnerability records
- CVSS information
- Affected product information
- References and advisories

### CISA Known Exploited Vulnerabilities

https://www.cisa.gov/known-exploited-vulnerabilities-catalog

**Purpose:** Catalog of vulnerabilities known to have been exploited in the wild.

Useful for vulnerability prioritization.

### Exploit Database

https://www.exploit-db.com/

**Purpose:** Public exploit research and proof-of-concept references.

Review code carefully before running it.

---

## 3. Security Frameworks

### MITRE ATT&CK

https://attack.mitre.org/

**Purpose:** Knowledge base of adversary tactics and techniques.

### MITRE CWE

https://cwe.mitre.org/

**Purpose:** Classification of software weakness types.

Examples:

```text
CWE-79  Cross-Site Scripting
CWE-89  SQL Injection
CWE-22  Path Traversal
```

### OWASP

https://owasp.org/

**Purpose:** Application-security education, standards, and community projects.

---

## 4. Web Security

### PortSwigger Academy

https://portswigger.net/web-security

**Purpose:** Web-security training and interactive labs.

### OWASP Web Security Testing Guide

https://owasp.org/www-project-web-security-testing-guide/

**Purpose:** Structured web-application security testing methodology.

### OWASP Cheat Sheet Series

https://cheatsheetseries.owasp.org/

**Purpose:** Secure implementation guidance.

### HackTricks

https://book.hacktricks.wiki/

**Purpose:** Practical penetration-testing reference.

---

## 5. Practical Labs

### Hack The Box

https://www.hackthebox.com/

### Hack The Box Academy

https://academy.hackthebox.com/

### TryHackMe

https://tryhackme.com/

### picoCTF

https://picoctf.org/

### OverTheWire

https://overthewire.org/wargames/

### VulnHub

https://www.vulnhub.com/

### PortSwigger Academy

https://portswigger.net/web-security

Use these platforms for authorized hands-on training.

---

## 6. Network Security

### Nmap

https://nmap.org/

**Purpose:** Network scanning documentation and reference material.

### Wireshark

https://www.wireshark.org/

**Purpose:** Packet analysis software and documentation.

### IANA

https://www.iana.org/

**Purpose:** Protocol parameters, port registries, and internet coordination information.

### RFC Editor

https://www.rfc-editor.org/

**Purpose:** Technical specifications for internet protocols.

---

## 7. Active Directory and Windows

### Microsoft Learn

https://learn.microsoft.com/

**Purpose:** Windows, PowerShell, Active Directory, and Microsoft cloud documentation.

### SpecterOps

https://specterops.io/

**Purpose:** Identity security and adversary research.

### BloodHound

https://bloodhound.specterops.io/

**Purpose:** Identity attack-path analysis documentation.

### LOLBAS

https://lolbas-project.github.io/

**Purpose:** Security-relevant behaviors of legitimate Windows binaries.

---

## 8. Linux Security

### Linux Manual Pages

https://man7.org/linux/man-pages/

**Purpose:** Linux system-call and command documentation.

### GTFOBins

https://gtfobins.github.io/

**Purpose:** Unix executable security behaviors.

### Linux Kernel

https://www.kernel.org/

**Purpose:** Kernel releases and development information.

---

## 9. Cloud Security

### AWS Documentation

https://docs.aws.amazon.com/

### Microsoft Azure Documentation

https://learn.microsoft.com/azure/

### Google Cloud Documentation

https://cloud.google.com/docs

### Kubernetes Documentation

https://kubernetes.io/docs/

### Docker Documentation

https://docs.docker.com/

Focus on:

```text
IAM
Network Security
Storage Permissions
Logging
Secrets Management
Container Security
```

---

## 10. Security Research

### Google Project Zero

https://googleprojectzero.blogspot.com/

### Trail of Bits

https://blog.trailofbits.com/

### PortSwigger Research

https://portswigger.net/research

### NCC Group Research

https://www.nccgroup.com/research-blog/

### Assetnote Research

https://www.assetnote.io/resources/research

These sources provide technical vulnerability research and root-cause analysis.

---

## 11. Threat Intelligence

### CISA

https://www.cisa.gov/

### VirusTotal

https://www.virustotal.com/

### MalwareBazaar

https://bazaar.abuse.ch/

### URLhaus

https://urlhaus.abuse.ch/

### AlienVault OTX

https://otx.alienvault.com/

Important: do not upload confidential files or sensitive data to public analysis services without authorization.

---

## 12. AI Security

### OWASP GenAI Security Project

https://genai.owasp.org/

**Focus:** LLM application security and agentic risks.

### MITRE ATLAS

https://atlas.mitre.org/

**Focus:** Adversarial machine-learning threats.

### NIST AI Risk Management Framework

https://www.nist.gov/itl/ai-risk-management-framework

**Focus:** AI risk governance and management.

### AI Security Papers

https://arxiv.org/

**Focus:** Academic and preprint research.

Preprints should be evaluated critically because they may not have undergone peer review.

---

## 13. Security News

### The Hacker News

https://thehackernews.com/

### BleepingComputer

https://www.bleepingcomputer.com/

### Krebs on Security

https://krebsonsecurity.com/

### Dark Reading

https://www.darkreading.com/

Use news sites for awareness, then verify technical claims against primary advisories and research.

---

## 14. Source Selection

| Question | Preferred Source |
|---|---|
| Is a CVE real? | CVE Program, vendor advisory |
| Is it exploited in the wild? | CISA KEV, credible threat intelligence |
| How does a protocol work? | RFC, official documentation |
| How does a vulnerability work? | Technical research, vendor advisory |
| How do I fix a web vulnerability? | OWASP Cheat Sheets |
| How do I learn practical testing? | PortSwigger, HTB, TryHackMe |
| How do I assess AI threats? | OWASP GenAI, MITRE ATLAS |

---

## 15. Research Workflow

```text
Security Question
       ↓
Find Primary Source
       ↓
Cross-Check References
       ↓
Understand Technical Details
       ↓
Reproduce Safely
       ↓
Document Conclusions
```

---

## Key Takeaway

The best cybersecurity research uses:

```text
AUTHORITATIVE SOURCES
        +
TECHNICAL EVIDENCE
        +
INDEPENDENT VALIDATION
```

A search result or social-media claim should not replace vendor documentation or reproducible research.
