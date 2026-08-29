# ⬆️ Privilege Escalation

> Privilege Escalation (PrivEsc) is the process of moving from a lower-privileged security context to a more privileged one after obtaining authorized access to a system.

---

## Overview

Initial access does not necessarily mean full control.

You may obtain a shell as:

```text
www-data
service
developer
standard-user
```

while administrative access belongs to:

```text
root           → Linux
Administrator  → Windows
SYSTEM         → Windows
```

The next objective in an authorized lab or assessment may therefore be:

```text
Initial Foothold
       ↓
Local Enumeration
       ↓
Identify Weakness
       ↓
Validate PrivEsc Path
       ↓
Higher Privilege
```

---

## Why Privilege Escalation Matters

Low-privileged access may restrict:

- Sensitive files
- Security configuration
- Credentials
- Services
- Other user data
- Administrative functions

Privilege escalation demonstrates whether an initial compromise can become significantly more severe.

---

## Vertical vs Horizontal Escalation

### Vertical

Move to a more privileged account.

```text
Linux:

www-data
   ↓
root
```

```text
Windows:

User
 ↓
Administrator / SYSTEM
```

### Horizontal

Access another account at a similar privilege level.

```text
User A
 ↓
User B
```

Both matter, although this module primarily focuses on **vertical local privilege escalation**.

---

## Module Structure

```text
05-Privilege-Escalation/

├── README.md
├── Linux.md
├── Windows.md
├── SUID.md
├── Capabilities.md
├── Cron-Jobs.md
├── Services.md
├── Docker.md
├── GTFOBins.md
├── LOLBAS.md
├── LinPEAS.md
└── WinPEAS.md
```

---

## Common Linux PrivEsc Areas

```text
sudo
SUID / SGID
Capabilities
Cron Jobs
Writable Files
Services
PATH
Credentials
Groups
Docker
Kernel / Software
```

---

## Common Windows PrivEsc Areas

```text
Services
File / Registry Permissions
Scheduled Tasks
Credentials
Tokens / Privileges
PATH Issues
Installer Policies
Application Configuration
Software / OS Vulnerabilities
```

---

## Enumeration First

Never begin with:

```text
Run exploit
```

Begin with:

```text
Who am I?
What groups am I in?
What OS/version?
What processes/services exist?
What permissions do I have?
What scheduled tasks exist?
What credentials/configurations are exposed?
```

---

## Manual Before Automated

Automated tools such as:

```text
LinPEAS
WinPEAS
```

are useful.

But first understand the environment manually.

Otherwise their output becomes:

```text
500 colored lines
```

instead of actionable information.

---

## PrivEsc Methodology

```text
Initial Access
     ↓
User / Groups
     ↓
OS / Architecture
     ↓
Permissions
     ↓
Processes / Services
     ↓
Scheduled Execution
     ↓
Credentials
     ↓
Special Privileges
     ↓
Software / Kernel
     ↓
Form Hypothesis
     ↓
Validate Safely
```

---

## Configuration vs Vulnerability

Many privilege-escalation paths are not traditional software CVEs.

Examples:

```text
Writable root script
Overly broad sudo rule
Weak service permissions
Exposed credentials
Privileged group membership
```

These are often **configuration or permission weaknesses**.

---

## Kernel Exploits

Kernel vulnerabilities can sometimes provide local privilege escalation.

However, kernel exploitation can:

- Crash the host
- Corrupt state
- Be highly version-specific

Therefore:

```text
Configuration-based paths
```

should generally be investigated before risky kernel exploitation.

---

## Evidence

When demonstrating PrivEsc, record:

```text
Initial User
Initial Groups
Weakness
Privilege Boundary Crossed
Final Security Context
Relevant Commands/Output
Remediation
```

Avoid unnecessary actions after the escalation has been proven.

---

## Red Team Perspective

Good PrivEsc is not:

> Run LinPEAS → copy first red line → run exploit.

It is:

```text
Understand System
      ↓
Identify Trust Boundary
      ↓
Find Misconfiguration
      ↓
Understand Why It Works
      ↓
Validate
```

The important skill is recognizing **what privileged process trusts something you can control**.

---

## Common Beginner Mistakes

- Running kernel exploits immediately.
- Trusting automated enumeration blindly.
- Ignoring group membership.
- Ignoring application configuration.
- Failing to inspect scheduled tasks.
- Treating every SUID binary as vulnerable.
- Confusing local PrivEsc with lateral movement.
- Continuing privileged actions after enough evidence has been collected.

---

## Quick Revision

```text
Linux:
sudo
SUID
Capabilities
Cron
Services
Groups
Credentials
Docker

Windows:
Services
Tasks
Privileges
Registry
Credentials
Permissions
Software

Rule:

Enumerate
 ↓
Understand
 ↓
Hypothesize
 ↓
Validate
```

---

## Practice

Recommended isolated environments:

- TryHackMe PrivEsc rooms
- Hack The Box Academy
- Hack The Box
- VulnHub
- Metasploitable
- Windows/Linux VMs you control

---

## References

- GTFOBins
- LOLBAS
- HackTricks
- Microsoft Security Documentation
- Linux Documentation
- Hack The Box Academy
