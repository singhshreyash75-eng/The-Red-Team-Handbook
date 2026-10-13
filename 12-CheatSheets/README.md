
# CheatSheets

> Practical command references and security-testing workflows for Linux, Windows, networking, Active Directory, web security, and authorized penetration testing.

## Module Overview

This module provides concise, command-oriented references for common cybersecurity tasks.

Unlike theory-focused modules, these cheat sheets emphasize:

```text
Command
   ↓
Purpose
   ↓
Example
   ↓
Interpretation
```

---

## Files

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

---

## Cheat Sheet Index

| File | Main Focus |
|---|---|
| Active-Directory.md | AD, Kerberos, LDAP, domain enumeration |
| Burp.md | HTTP interception and web testing |
| File-Transfer.md | SCP, SFTP, rsync, HTTP, SMB |
| Linux.md | Linux commands, processes, permissions |
| Networking.md | TCP/IP, DNS, ports, diagnostics |
| Nmap.md | Host discovery and service enumeration |
| One-Liners.md | Quick Bash, Python, PowerShell commands |
| PrivEsc.md | Linux/Windows privilege reviews |
| Reverse-Shell.md | Reverse connections and shell concepts |
| SQLi.md | SQL injection concepts and validation |
| Windows.md | Windows administration and security |
| XSS.md | Cross-site scripting concepts and testing |

---

## 1. Reconnaissance Workflow

```text
Authorized Scope
      ↓
Asset Discovery
      ↓
DNS Validation
      ↓
Service Discovery
      ↓
Technology Fingerprinting
      ↓
Application Mapping
```

Common tools:

```text
Amass
Subfinder
Assetfinder
Nmap
httpx
Katana
```

---

## 2. Network Enumeration

Important commands:

```bash
nmap -sV 192.0.2.10
```

```bash
dig example.com
```

```bash
curl -I https://example.com
```

```bash
nc -vz 192.0.2.10 443
```

---

## 3. Web Application Testing

Core workflow:

```text
Browser
   ↓
Burp / Caido
   ↓
Capture Request
   ↓
Modify Controlled Input
   ↓
Replay
   ↓
Validate Response
```

Important testing categories:

```text
Authentication
Authorization
Input validation
Session management
Business logic
API security
```

---

## 4. Linux Security

Focus areas:

```text
Users
Groups
File permissions
sudo
SUID / SGID
Capabilities
Services
Cron
Network listeners
```

---

## 5. Windows Security

Focus areas:

```text
Users
Groups
Privileges
Services
Scheduled tasks
File ACLs
PowerShell
Windows logs
Active Directory
```

---

## 6. Active Directory

Important concepts:

```text
Domains
Forests
Domain Controllers
Kerberos
LDAP
SPNs
ACLs
GPOs
Trusts
```

Core question:

```text
Which identity can control
which resource?
```

---

## 7. Privilege Escalation

Conceptual workflow:

```text
Low-Privileged Identity
        ↓
Enumerate Permissions
        ↓
Identify Misconfiguration
        ↓
Verify Security Boundary
        ↓
Document Risk
```

---

## 8. SQL Injection

SQL injection occurs when untrusted input changes database-query structure.

Secure coding should use:

```text
Parameterized queries
Input validation
Least-privileged database accounts
```

---

## 9. Cross-Site Scripting

XSS occurs when untrusted content is interpreted as executable browser code.

Important categories:

```text
Reflected
Stored
DOM-Based
```

Defenses:

```text
Context-aware output encoding
Safe DOM APIs
HTML sanitization
CSP as defense in depth
```

---

## 10. Network Shells

Important concepts:

```text
Bind connection
Reverse connection
TCP
TTY / PTY
Authentication
Encryption
```

Network shells should be used only in explicitly authorized isolated environments.

---

## 11. File Transfer

Preferred secure methods:

```text
SCP
SFTP
rsync over SSH
HTTPS
```

Always verify:

```text
Source
Destination
Permissions
Integrity
Confidentiality
```

---

## 12. Common Security Tools

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
Hashcat
John the Ripper
```

Select tools based on the question being investigated rather than tool popularity.

---

## 13. Security Assessment Methodology

```text
SCOPE
  ↓
DISCOVER
  ↓
ENUMERATE
  ↓
ANALYZE
  ↓
VALIDATE
  ↓
DOCUMENT
  ↓
REMEDIATE
  ↓
RETEST
```

---

## 14. Evidence Collection

Record:

```text
Affected asset
Timestamp
Tool and version
Relevant command
Observed output
Security impact
Remediation
```

Never unnecessarily expose passwords, tokens, or personal information.

---

## 15. Safety and Authorization

All active security testing must follow:

```text
Explicit authorization
Defined scope
Rules of engagement
Rate limits
Data-handling requirements
Service availability constraints
```

A discovered asset is not automatically authorized for testing.

---

## Key Takeaway

These cheat sheets are intended to make common security workflows:

```text
FASTER
  +
REPEATABLE
  +
STRUCTURED
  +
EASIER TO VALIDATE
```

**Understand the command and its impact before executing it.**
