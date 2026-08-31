# Active Directory

> Active Directory security notes covering fundamentals, enumeration, Kerberos, credential attacks, graph analysis, and common assessment tools.

## Module Overview

This module focuses on understanding how **Active Directory identities, authentication mechanisms, permissions, and relationships** create security boundaries.

Active Directory should be viewed as:

```text
Users
   +
Groups
   +
Computers
   +
Credentials
   +
Permissions
   +
Relationships
       ↓
Active Directory Security
```

---

# Files

## Basics

```text
Basics.md
```

Covers:

```text
Domains
Forests
Domain Controllers
Users
Groups
OUs
GPOs
SIDs
LDAP
SMB
Kerberos
NTLM
```

---

## AS-REP Roasting

```text
AS-REP-Roasting.md
```

Core concept:

```text
Kerberos preauthentication disabled
             ↓
AS-REP material obtainable
             ↓
Offline password auditing
```

---

## Kerberoasting

```text
Kerberoasting.md
```

Core concept:

```text
Service Account
      +
     SPN
      ↓
Service Ticket
      ↓
Offline password auditing
```

---

## BloodHound

```text
BloodHound.md
```

Analyzes AD as a graph:

```text
Objects
   +
Relationships
   ↓
Privilege Paths
```

---

## SharpHound

```text
SharpHound.md
```

Collector used to gather AD relationship information for BloodHound analysis.

```text
Active Directory
       ↓
SharpHound
       ↓
Collected Data
       ↓
BloodHound
```

---

## Impacket

```text
Impacket.md
```

Python toolkit implementing Windows/network protocols.

Important areas:

```text
SMB
RPC
NTLM
Kerberos
Remote administration
AD security testing
```

---

## Mimikatz

```text
Mimikatz.md
```

Windows authentication and credential-security research tool.

Important concepts:

```text
LSASS
NTLM hashes
Kerberos tickets
Credential material
Golden Tickets
Silver Tickets
```

---

## Rubeus

```text
Rubeus.md
```

Windows-focused Kerberos interaction and security-assessment tool.

Important topics:

```text
Kerberos tickets
AS-REP
TGS
Kerberoasting
Ticket inspection
Delegation
```

---

## Pass-the-Hash

```text
Pass-the-Hash.md
```

Concept:

```text
NT Hash
   ↓
Authentication material
   ↓
Authenticate without recovering
plaintext password
```

---

## Golden Ticket

```text
Golden-Ticket.md
```

Concept:

```text
KRBTGT secret
      ↓
Forged TGT
      ↓
Domain-level Kerberos impact
```

---

## Silver Ticket

```text
Silver-Ticket.md
```

Concept:

```text
Service-account key
        ↓
Forged service ticket
        ↓
Service-specific Kerberos impact
```

---

# Learning Order

Recommended sequence:

```text
1. Basics
      ↓
2. Authentication fundamentals
      ↓
3. Enumeration
      ↓
4. BloodHound / SharpHound
      ↓
5. Kerberoasting
      ↓
6. AS-REP Roasting
      ↓
7. Credential concepts
      ↓
8. Pass-the-Hash
      ↓
9. Kerberos ticket concepts
      ↓
10. Golden / Silver Tickets
```

---

# Core Protocols

Memorize:

```text
53      DNS
88      Kerberos
135     RPC
389     LDAP
445     SMB
464     Kerberos password operations
636     LDAPS
3268    Global Catalog
3269    Global Catalog TLS
```

---

# Core Enumeration

Windows identity:

```cmd
whoami
whoami /all
whoami /groups
whoami /priv
```

Host:

```cmd
hostname
systeminfo
ipconfig /all
```

Domain:

```cmd
echo %USERDOMAIN%
echo %LOGONSERVER%
```

Users/groups:

```cmd
net user /domain
net group /domain
```

Kerberos tickets:

```cmd
klist
```

Domain Controller discovery:

```cmd
nltest /dsgetdc:<domain>
```

---

# Kerberos Mental Model

```text
             KDC
              |
       +------+------+
       |             |
      AS            TGS
       |             |
       v             v
      TGT      Service Ticket
```

Authentication flow:

```text
User
 ↓
AS-REQ
 ↓
KDC
 ↓
AS-REP / TGT
 ↓
TGS-REQ
 ↓
Service Ticket
 ↓
Service
```

---

# NTLM Mental Model

```text
Client
   |
   | request
   v
Server
   |
   | challenge
   v
Client
   |
   | response
   v
Server
```

---

# AD Attack-Path Mindset

Do not only search for:

```text
"Who is Domain Admin?"
```

Instead ask:

```text
Who controls whom?
Who can log into what?
Who belongs to which group?
Which machine contains which session?
Who can modify which AD object?
Which credential can access which system?
```

Example:

```text
Low-Privilege User
        |
     MemberOf
        |
        v
      Group
        |
     AdminTo
        |
        v
   Workstation
        |
    HasSession
        |
        v
Privileged User
```

This is why AD is best understood as a **graph of relationships**.

---

# Attack Concepts Map

```text
                 Active Directory
                        |
        +---------------+---------------+
        |                               |
    Kerberos                          NTLM
        |                               |
   +----+------+                   Pass-the-Hash
   |           |
AS-REP      Service Tickets
Roasting        |
                |
          Kerberoasting
                |
         Ticket concepts
           /         \
      Golden       Silver
      Ticket       Ticket
```

---

# Tools Map

```text
BloodHound
    ↓
Graph analysis

SharpHound
    ↓
AD data collection


Impacket
    ↓
Windows/network protocols


Mimikatz
    ↓
Windows credentials +
Kerberos internals


Rubeus
    ↓
Kerberos-focused operations
```

---

# Security Methodology

Use a structured approach:

```text
1. Identify current context
           ↓
2. Enumerate domain
           ↓
3. Enumerate users/groups/computers
           ↓
4. Understand authentication
           ↓
5. Map relationships
           ↓
6. Identify privilege boundaries
           ↓
7. Validate findings
           ↓
8. Determine security impact
```

---

# Important Principle

A finding is not automatically an exploitable vulnerability.

Always distinguish:

```text
Interesting configuration
          ↓
Potential attack path
          ↓
Manual verification
          ↓
Confirmed security impact
```

---

# Defensive Priorities

Strong AD security requires:

```text
Least privilege
Strong credential management
Administrative tiering
Secure service accounts
Kerberos hardening
Reduced NTLM usage
Protected Domain Controllers
Secure ACLs
Monitoring
Endpoint protection
Regular privilege review
```

---

# Key Takeaway

Active Directory security can be summarized as:

```text
IDENTITY
   +
AUTHENTICATION
   +
AUTHORIZATION
   +
RELATIONSHIPS
   =
AD SECURITY
```

The goal of this module is to understand not just individual attacks or tools, but **why an AD relationship creates or prevents a privilege path**.
