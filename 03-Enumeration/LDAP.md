# 📚 LDAP Enumeration

> LDAP (Lightweight Directory Access Protocol) is used to query and manage directory services. It is particularly important in Microsoft Active Directory, where LDAP exposes structured information about users, groups, computers, organizational units, and other directory objects.

---

## Overview

LDAP commonly appears on:

```text
389/TCP → LDAP
636/TCP → LDAP over TLS (LDAPS)
```

In Active Directory environments you may also encounter LDAP-related communication on additional ports depending on the service architecture.

For this chapter, focus on:

```text
389
636
```

We'll go much deeper during the Active Directory module.

---

# What is a Directory?

A directory is a structured collection of objects.

Example:

```text
Company
│
├── Users
│   ├── Alice
│   └── Bob
│
├── Groups
│   ├── IT
│   └── Finance
│
└── Computers
    ├── PC01
    └── SERVER01
```

LDAP provides a standardized way to query and modify directory information.

---

# LDAP vs Database

LDAP is not simply another SQL database.

Directory services are optimized for hierarchical information and frequent reads.

Example:

```text
Domain
  ↓
Organizational Units
  ↓
Users / Groups / Computers
```

---

# LDAP in Active Directory

Microsoft Active Directory uses LDAP as one of its core protocols.

Conceptually:

```text
Active Directory
      │
      ├── Users
      ├── Groups
      ├── Computers
      ├── Policies
      └── Organizational Units
             ↑
            LDAP
```

This makes LDAP one of the most important protocols for AD enumeration.

---

# Distinguished Names (DN)

LDAP objects are identified using **Distinguished Names**.

Example:

```text
CN=Alice,OU=Users,DC=example,DC=com
```

Breakdown:

```text
CN → Common Name
OU → Organizational Unit
DC → Domain Component
```

---

# Domain Example

Domain:

```text
corp.example.com
```

LDAP base DN:

```text
DC=corp,DC=example,DC=com
```

Understanding this conversion is important.

---

# Common LDAP Components

| Component | Meaning |
|---|---|
| CN | Common Name |
| OU | Organizational Unit |
| DC | Domain Component |

Example:

```text
CN=John Doe,
OU=Employees,
DC=example,
DC=com
```

---

# Initial Nmap Enumeration

Authorized/lab example:

```bash
nmap -sV -p 389,636 TARGET
```

Default scripts:

```bash
nmap -sC -sV -p 389,636 TARGET
```

If LDAP appears alongside:

```text
53
88
135
389
445
464
```

you may be looking at a Windows Domain Controller.

Do not rely on ports alone—correlate the evidence.

---

# LDAP Root DSE

LDAP servers expose a special entry called:

```text
Root DSE
```

It can provide information about the directory server.

Potential attributes include:

```text
namingContexts
defaultNamingContext
supportedLDAPVersion
```

This can help identify the base DN.

---

# ldapsearch

`ldapsearch` is a common Linux command-line LDAP client.

General syntax:

```bash
ldapsearch \
-H ldap://TARGET \
-x \
-b "BASE_DN"
```

`-x` requests simple authentication mode rather than SASL.

The exact authentication requirements depend on the target.

---

# Query Root DSE

In an authorized lab where anonymous/root-DSE queries are permitted:

```bash
ldapsearch -x -H ldap://TARGET -s base
```

A useful attribute to look for is:

```text
defaultNamingContext
```

Example:

```text
DC=example,DC=local
```

---

# Base DN

Once identified:

```text
DC=example,DC=local
```

can become the base for subsequent authorized directory searches.

Conceptually:

```bash
ldapsearch \
-x \
-H ldap://TARGET \
-b "DC=example,DC=local"
```

Whether anonymous enumeration succeeds depends on server policy.

Modern AD environments generally require authentication for substantial directory information.

---

# LDAP Filters

LDAP searches use filters.

Conceptually:

```text
(objectClass=user)
```

or:

```text
(objectClass=computer)
```

or:

```text
(objectClass=group)
```

Filters allow searches to focus on specific object types.

---

# User Objects

In Active Directory, user objects may contain attributes such as:

```text
sAMAccountName
userPrincipalName
displayName
memberOf
```

The exact information visible depends on authentication and directory permissions.

---

# Computer Objects

Computer records can help map domain systems.

Potential attributes:

```text
name
dNSHostName
operatingSystem
```

This becomes extremely useful during authenticated AD enumeration.

---

# Groups

Groups define collections of accounts and are heavily used for authorization.

Examples:

```text
Domain Users
IT
Helpdesk
Administrators
```

Group membership becomes critically important during privilege-path analysis.

---

# Organizational Units

OUs organize directory objects.

Example:

```text
Company
├── Servers
├── Workstations
├── Finance
└── IT
```

Group Policy Objects can be linked to sites, domains, and OUs.

We'll cover that later in Active Directory.

---

# Anonymous LDAP Bind

Some LDAP servers may allow limited anonymous queries.

Conceptually:

```text
Client
  ↓
Anonymous Bind
  ↓
LDAP Server
  ↓
Limited Information
```

Whether this is allowed depends on configuration.

Do not assume anonymous LDAP is available in modern Active Directory.

---

# Authenticated LDAP

With authorized credentials, directory enumeration can become much richer.

Conceptually:

```text
Domain Credentials
       ↓
LDAP
       ↓
Users
Groups
Computers
Policies
Relationships
```

Tools such as BloodHound/SharpHound later use directory information to help analyze AD relationships.

---

# LDAP vs LDAPS

### LDAP

```text
389/TCP
```

### LDAPS

```text
636/TCP
```

LDAPS protects LDAP communication using TLS.

LDAP can also use mechanisms such as StartTLS depending on deployment.

---

# TLS Inspection

For an authorized LDAPS service:

```bash
openssl s_client -connect TARGET:636
```

This can help inspect:

- Certificate
- Hostnames
- Issuer
- TLS configuration

---

# LDAP + DNS

Active Directory depends heavily on DNS.

Typical relationship:

```text
DNS
 ↓
Find Domain Controller
 ↓
LDAP
 ↓
Enumerate Directory
```

This is why your DNS fundamentals and enumeration notes matter.

---

# LDAP + SMB

LDAP may reveal:

```text
Users
Groups
Computers
```

SMB may reveal:

```text
Shares
Files
Host information
```

Together they provide a much stronger picture of the Windows environment.

---

# LDAP + Kerberos

In Active Directory:

```text
LDAP
   ↓
Directory Information

Kerberos
   ↓
Authentication
```

These protocols work together as part of the domain environment.

We'll cover Kerberos-related techniques in Module 06.

---

# Useful Tools

Common LDAP/AD tools include:

```text
ldapsearch
Nmap
NetExec
ldapdomaindump
BloodHound
SharpHound
```

The latter tools will be covered in greater depth during Active Directory.

---

# Enumeration Workflow

```text
389 / 636
     ↓
Identify LDAP
     ↓
Root DSE
     ↓
Determine Base DN
     ↓
Determine Authentication Requirements
     ↓
Authorized Directory Queries
     ↓
Users
Groups
Computers
     ↓
Relationships
```

---

# Example Notes

```text
Host:
10.10.10.60

Ports:
389
636

Directory:
Active Directory

Base DN:
DC=lab,DC=local

Anonymous:
Root DSE only

Domain:
lab.local

Related Services:
DNS
Kerberos
SMB
```

---

# Red Team Perspective

LDAP is extremely valuable because it can transform:

```text
One Domain Controller
```

into:

```text
Domain
├── Users
├── Groups
├── Computers
├── Services
└── Relationships
```

During Active Directory assessments, relationships are often more important than individual vulnerabilities.

Example:

```text
User
 ↓
Group
 ↓
Permission
 ↓
Server
 ↓
Privilege
```

That concept becomes central when we reach BloodHound.

---

# Defensive Considerations

Administrators should:

- Restrict unnecessary anonymous access
- Use appropriate LDAP signing/channel-binding protections
- Prefer protected LDAP transport where required
- Apply least privilege
- Monitor unusual directory queries
- Secure privileged accounts

Exact AD hardening requirements depend on the Windows/domain version and architecture.

---

# Common Beginner Mistakes

- Treating LDAP as a normal SQL database.
- Forgetting the base DN.
- Assuming anonymous bind always works.
- Ignoring LDAPS.
- Enumerating users without understanding groups.
- Ignoring relationships between LDAP, DNS, Kerberos, and SMB.
- Jumping directly to BloodHound without understanding directory structure.

---

# Interview Questions

### What is LDAP?

A protocol for accessing and managing directory information.

### Default LDAP ports?

389 for LDAP and commonly 636 for LDAPS.

### What is a Distinguished Name?

The hierarchical identifier of an LDAP object.

### What does DC mean?

Domain Component.

### What does OU mean?

Organizational Unit.

### Why is LDAP important in Active Directory?

AD stores and exposes directory objects such as users, groups, and computers through LDAP.

### LDAP vs LDAPS?

LDAPS protects LDAP communication with TLS.

---

# Quick Revision

```text
Ports:

389 → LDAP
636 → LDAPS

DN Example:

CN=Alice,
OU=Users,
DC=example,
DC=com

Important:

Root DSE
Base DN
Users
Groups
Computers
OUs

Tools:

ldapsearch
Nmap
NetExec
ldapdomaindump
BloodHound
SharpHound
```

---

# Practice

- TryHackMe Active Directory labs
- Hack The Box Academy
- Windows Server AD lab
- GOAD (Game of Active Directory) when you reach the AD module

---

# References

- RFC 4511 — LDAP
- Microsoft Active Directory Documentation
- OpenLDAP Documentation
- BloodHound Documentation
- Hack The Box Academy
