# CVEs

> Vulnerability identification, scoring, research, exploit discovery, and validation reference.

## Module Overview

This module covers the complete workflow for researching known vulnerabilities.

```text
Software / Service
       ↓
Product + Version
       ↓
CVE Research
       ↓
Affected Version?
       ↓
CVSS / Severity
       ↓
Exploit Research
       ↓
Manual Validation
       ↓
Patch / Mitigation
```

---

# Files

```text
CVE-Basics.md
CVSS.md
Case-Studies.md
ExploitDB.md
MITRE.md
NVD.md
README.md
Searchsploit.md
```

---

# CVE

**CVE = Common Vulnerabilities and Exposures**

Example:

```text
CVE-2021-44228
```

Format:

```text
CVE-YEAR-ID
```

A CVE provides a standardized identifier for a specific publicly disclosed vulnerability.

Remember:

```text
CVE != Exploit
```

---

# CWE

**CWE = Common Weakness Enumeration**

CWE describes a category/class of weakness.

Conceptually:

```text
CWE
 |
Weakness Class
 |
 +--> CVE A
 +--> CVE B
 +--> CVE C
```

Therefore:

```text
CWE = Weakness category

CVE = Specific vulnerability
```

---

# CVSS

**CVSS = Common Vulnerability Scoring System**

Typical score range:

```text
0.0 - 10.0
```

Common qualitative ratings:

```text
0.0          None
0.1 - 3.9    Low
4.0 - 6.9    Medium
7.0 - 8.9    High
9.0 - 10.0   Critical
```

Important:

```text
CVSS != Complete Risk
```

Real risk also depends on:

```text
Exposure
Exploitability
Asset criticality
Threat activity
Business impact
Security controls
```

---

# CVSS Vector

Example:

```text
AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H
```

Important metrics:

```text
AV = Attack Vector
AC = Attack Complexity
PR = Privileges Required
UI = User Interaction
S  = Scope
C  = Confidentiality
I  = Integrity
A  = Availability
```

---

# NVD

**NVD = National Vulnerability Database**

Maintained by:

```text
NIST
```

NVD provides CVE-related enrichment such as:

```text
CVSS
CWE
CPE
References
Affected configurations
```

Conceptually:

```text
CVE
 ↓
NVD
 ↓
Additional Analysis
```

---

# MITRE

Important MITRE-related security knowledge bases include:

```text
CVE
CWE
ATT&CK
CAPEC
```

Remember:

```text
CVE
=
Specific vulnerability

CWE
=
Weakness category

ATT&CK
=
Adversary behavior

CAPEC
=
Attack patterns
```

---

# Exploit-DB

Exploit-DB provides public:

```text
Exploit code
Proofs of Concept
Security research
```

An Exploit-DB entry has:

```text
EDB-ID
```

Remember:

```text
EDB-ID != CVE-ID
```

---

# Searchsploit

Searchsploit provides a command-line interface for searching a local Exploit-DB dataset.

Example:

```bash
searchsploit apache
```

Search by CVE:

```bash
searchsploit CVE-XXXX-YYYY
```

---

# Vulnerability Research Workflow

Use:

```text
1. Identify service/product

2. Determine exact version

3. Search CVE databases

4. Read CVE/NVD information

5. Read vendor advisory

6. Determine affected versions

7. Examine CVSS vector

8. Check prerequisites

9. Research public PoCs

10. Review exploit code

11. Validate safely

12. Document evidence
```

---

# Version Detection

Examples:

```bash
nginx -v
apache2 -v
openssl version
ssh -V
```

Package managers:

```bash
dpkg -l
rpm -qa
```

Network banners can provide clues, but should not automatically be considered definitive.

---

# Backported Patches

A critical concept:

```text
Old-looking version
       ↓
Vendor backported security patch
       ↓
Potentially NOT vulnerable
```

Therefore:

```text
Version Match
     !=
Confirmed Vulnerability
```

Check vendor/distribution advisories.

---

# Exploit Research

Possible sources:

```text
Exploit-DB
Searchsploit
Vendor research
Security advisories
Research repositories
```

Never blindly execute public exploit code.

Use:

```text
Read
 ↓
Understand
 ↓
Verify
 ↓
Isolated Lab
 ↓
Test
```

---

# PoC

**PoC = Proof of Concept**

A PoC demonstrates vulnerability behavior.

It may be:

```text
Incomplete
Unreliable
Version-specific
Research-only
Potentially destructive
```

Therefore:

```text
PoC != Reliable Exploit
```

---

# Exploitability

A CVE may have:

```text
No public exploit

PoC only

Reliable public exploit

Active exploitation
```

These represent very different operational risk levels.

---

# CISA KEV

**KEV = Known Exploited Vulnerabilities**

CISA maintains a catalog of vulnerabilities known to be exploited according to its criteria.

This adds valuable exploitation context.

Conceptually:

```text
CVE
 +
CVSS
 +
CISA KEV
 +
Asset Exposure
 =
Better Prioritization
```

---

# EPSS

**EPSS = Exploit Prediction Scoring System**

Conceptually:

```text
CVSS
=
Technical severity
```

versus:

```text
EPSS
=
Probability-oriented exploitation estimate
```

They answer different questions.

---

# Case Study Lessons

Examples studied in this module include concepts demonstrated by:

```text
Log4Shell
Heartbleed
Shellshock
EternalBlue
Dirty COW
BlueKeep
Baron Samedit
```

Do not memorize only CVE numbers.

Understand:

```text
Vulnerability Class
       +
Prerequisites
       +
Impact
       +
Mitigation
```

---

# Vulnerability Types

Important categories:

```text
Remote Code Execution
Local Privilege Escalation
Authentication Bypass
Information Disclosure
SQL Injection
Command Injection
Path Traversal
Memory Corruption
Race Condition
Denial of Service
```

---

# Remote vs Local

Remote:

```text
Attacker
   |
Network
   |
Vulnerability
```

Local:

```text
Existing local access
       |
Vulnerability
       |
Privilege / Impact
```

This distinction affects severity and attack chaining.

---

# Attack Chains

A single vulnerability may be only one part of a compromise.

Example:

```text
Web RCE
   ↓
Low-Privilege Shell
   ↓
Local Privilege Escalation
   ↓
Root
   ↓
Credential Access
   ↓
Lateral Movement
```

Always analyze vulnerabilities in environmental context.

---

# Scanner Findings

Never assume:

```text
Scanner says CVE
       =
Confirmed Vulnerability
```

Use:

```text
Scanner
   ↓
Potential Finding
   ↓
Manual Research
   ↓
Version Verification
   ↓
Configuration Verification
   ↓
Safe Validation
   ↓
Confirmed Finding
```

---

# Research Sources

A strong investigation combines:

```text
CVE Program
NVD
Vendor Advisory
CISA
CERT
Exploit-DB
Technical Research
```

No single source tells the entire story.

---

# Documentation Template

```text
CVE:
CVE-XXXX-YYYY

Product:
__________

Version:
__________

Affected:
Yes / No / Unknown

CVSS:
__________

Attack Vector:
__________

Authentication:
__________

Impact:
__________

Public PoC:
__________

Known Exploitation:
__________

Patch:
__________

Mitigation:
__________

Evidence:
__________
```

---

# Key Takeaway

The complete mindset is:

```text
DISCOVER
   ↓
IDENTIFY
   ↓
RESEARCH
   ↓
VERIFY
   ↓
VALIDATE
   ↓
REMEDIATE
```

Never:

```text
Version
  +
Google Search
  =
Automatically Vulnerable
```

Instead:

```text
Product
   +
Version
   +
Patch Status
   +
Configuration
   +
Prerequisites
   +
Evidence
   =
Confirmed Vulnerability
```
