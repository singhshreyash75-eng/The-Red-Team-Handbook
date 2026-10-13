
# Cybersecurity GitHub Repositories

> Curated open-source repositories for reconnaissance, web security, penetration testing, vulnerability research, Active Directory, cloud security, and AI red teaming.

## 1. Overview

GitHub is an important source of security tools, research, educational material, and technical documentation.

Repositories are useful for:

- Studying tool implementations
- Learning security automation
- Understanding vulnerability research
- Exploring attack-surface discovery
- Building practical security projects
- Reviewing defensive techniques

Recommended workflow:

```text
Find Repository
      ↓
Read Documentation
      ↓
Review Source Code
      ↓
Understand Dependencies
      ↓
Test in Authorized Lab
      ↓
Document Findings
```

---

## 2. Reconnaissance Repositories

### Subfinder

Repository: https://github.com/projectdiscovery/subfinder

**Purpose:** Passive subdomain enumeration.

Features:

- Multiple passive data sources
- API integrations
- Fast subdomain discovery
- Structured output

### Amass

Repository: https://github.com/owasp-amass/amass

**Purpose:** Attack-surface discovery and infrastructure mapping.

Features:

- DNS enumeration
- Asset discovery
- Infrastructure relationships
- External data-source integration

### httpx

Repository: https://github.com/projectdiscovery/httpx

**Purpose:** HTTP service probing and fingerprinting.

Features:

- Live host validation
- HTTP status codes
- Technology detection
- TLS information
- Web application discovery

### Katana

Repository: https://github.com/projectdiscovery/katana

**Purpose:** Web crawling and endpoint discovery.

Features:

- Web crawling
- JavaScript-aware discovery
- Endpoint extraction
- Modern application support

---

## 3. Web Security Repositories

### Nuclei

Repository: https://github.com/projectdiscovery/nuclei

**Purpose:** Template-based security checks.

Features:

- YAML-based templates
- Vulnerability signatures
- Misconfiguration checks
- Automated security testing

### Nuclei Templates

Repository: https://github.com/projectdiscovery/nuclei-templates

**Purpose:** Community-maintained security-check templates.

Important:

```text
Template Match
      !=
Confirmed Vulnerability
```

### FFUF

Repository: https://github.com/ffuf/ffuf

**Purpose:** HTTP fuzzing and content discovery.

### Feroxbuster

Repository: https://github.com/epi052/feroxbuster

**Purpose:** Recursive web-content discovery.

### Gobuster

Repository: https://github.com/OJ/gobuster

**Purpose:** Directory, DNS, and virtual-host enumeration.

---

## 4. Security References and Wordlists

### SecLists

Repository: https://github.com/danielmiessler/SecLists

**Purpose:** Security testing wordlists.

Categories:

```text
Discovery
Passwords
Usernames
Fuzzing
Payloads
```

### PayloadsAllTheThings

Repository: https://github.com/swisskyrepo/PayloadsAllTheThings

**Purpose:** Vulnerability-specific testing references and payload collections.

### HackTricks

Repository: https://github.com/HackTricks-wiki/hacktricks

**Purpose:** Penetration-testing techniques and security reference material.

### GTFOBins

Repository: https://github.com/GTFOBins/GTFOBins.github.io

**Purpose:** Security-relevant behaviors of Unix executables.

### LOLBAS

Repository: https://github.com/LOLBAS-Project/LOLBAS

**Purpose:** Documentation of legitimate Windows binaries that can be abused.

---

## 5. Active Directory Repositories

### BloodHound

Repository: https://github.com/SpecterOps/BloodHound

**Purpose:** Active Directory and identity attack-path analysis.

### SharpHound

Repository: https://github.com/SpecterOps/SharpHound

**Purpose:** Active Directory relationship collection.

### Impacket

Repository: https://github.com/fortra/impacket

**Purpose:** Python implementations of network protocols.

Relevant technologies:

```text
SMB
MSRPC
Kerberos
LDAP
NTLM
```

### NetExec

Repository: https://github.com/Pennyw0rth/NetExec

**Purpose:** Network security assessment and enumeration in authorized environments.

### Certipy

Repository: https://github.com/ly4k/Certipy

**Purpose:** Active Directory Certificate Services security assessment.

---

## 6. Privilege Escalation Repositories

### PEASS-ng

Repository: https://github.com/peass-ng/PEASS-ng

**Purpose:** Privilege-escalation enumeration.

Includes tools such as:

```text
LinPEAS
WinPEAS
```

### Seatbelt

Repository: https://github.com/GhostPack/Seatbelt

**Purpose:** Windows security enumeration and situational awareness.

### PowerUp

Repository: https://github.com/PowerShellMafia/PowerSploit

**Purpose:** PowerShell security assessment framework containing privilege-review functionality.

Review project maintenance status before relying on older tools.

---

## 7. Reverse Engineering Repositories

### Ghidra

Repository: https://github.com/NationalSecurityAgency/ghidra

**Purpose:** Software reverse engineering.

Features:

```text
Disassembly
Decompilation
Binary Analysis
Scripting
```

### radare2

Repository: https://github.com/radareorg/radare2

**Purpose:** Binary analysis and reverse-engineering framework.

### pwndbg

Repository: https://github.com/pwndbg/pwndbg

**Purpose:** GDB enhancements for debugging and exploit-development research.

---

## 8. Cloud Security Repositories

### Prowler

Repository: https://github.com/prowler-cloud/prowler

**Purpose:** Cloud security assessment and configuration auditing.

### ScoutSuite

Repository: https://github.com/nccgroup/ScoutSuite

**Purpose:** Multi-cloud security auditing.

### CloudSplaining

Repository: https://github.com/salesforce/cloudsplaining

**Purpose:** AWS IAM privilege analysis.

### Trivy

Repository: https://github.com/aquasecurity/trivy

**Purpose:** Vulnerability, configuration, and secret scanning.

---

## 9. Container and Kubernetes Security

### kube-bench

Repository: https://github.com/aquasecurity/kube-bench

**Purpose:** Kubernetes configuration auditing against CIS benchmarks.

### kube-hunter

Repository: https://github.com/aquasecurity/kube-hunter

**Purpose:** Kubernetes security assessment.

### Falco

Repository: https://github.com/falcosecurity/falco

**Purpose:** Runtime security monitoring.

---

## 10. AI and LLM Security Repositories

### garak

Repository: https://github.com/NVIDIA/garak

**Purpose:** LLM vulnerability scanning and security evaluation.

### PyRIT

Repository: https://github.com/Azure/PyRIT

**Purpose:** Generative AI red-teaming framework.

### promptfoo

Repository: https://github.com/promptfoo/promptfoo

**Purpose:** LLM application testing, evaluations, and red teaming.

### ART — Adversarial Robustness Toolbox

Repository: https://github.com/Trusted-AI/adversarial-robustness-toolbox

**Purpose:** Adversarial machine-learning security research.

### Giskard

Repository: https://github.com/Giskard-AI/giskard-oss

**Purpose:** AI/ML model evaluation and security testing.

---

## 11. Repository Evaluation Checklist

Before using an open-source security tool:

```text
[ ] Verify repository ownership
[ ] Read documentation
[ ] Review license
[ ] Check recent maintenance
[ ] Inspect dependencies
[ ] Review installation scripts
[ ] Check known issues
[ ] Test in isolated environment
[ ] Understand network behavior
```

Security tools themselves introduce supply-chain risk.

---

## 12. Recommended Learning Strategy

Do not only install repositories.

Study their architecture.

Example:

```text
Tool
 ↓
Input Handling
 ↓
Core Logic
 ↓
Networking
 ↓
Parsing
 ↓
Output
```

Understanding implementations helps develop independent security tooling skills.

---

## Key Takeaway

GitHub repositories provide:

```text
TOOLS
  +
SOURCE CODE
  +
DOCUMENTATION
  +
RESEARCH
```

The greatest value comes from understanding how security tools work internally, not simply memorizing their command-line options.
