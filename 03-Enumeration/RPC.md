# 🔄 RPC Enumeration

> Remote Procedure Call (RPC) allows software on one computer to invoke procedures or services on another computer. RPC appears in both Unix/Linux and Windows environments and can expose valuable service, host, domain, and account information depending on configuration.

---

## Overview

RPC is not one single application.

It is a **communication mechanism** used by many services.

You may encounter:

```text
SunRPC / rpcbind
Microsoft RPC
SMB Named Pipes
NFS-related RPC
```

This distinction is important.

---

# Common Ports

### Unix/Linux rpcbind

```text
111/TCP
111/UDP
```

### Microsoft RPC Endpoint Mapper

```text
135/TCP
```

### SMB-related RPC

Often accessible through:

```text
445/TCP
```

Windows RPC can also use dynamically assigned high ports.

---

# Why RPC Matters

RPC enumeration can reveal:

- Available RPC services
- NFS-related services
- Windows host/domain information
- Users
- Groups
- Domain policies
- Network services

The exact exposure depends heavily on platform and permissions.

---

# Unix RPC Architecture

Simplified:

```text
Client
   ↓
rpcbind
   ↓
Service Mapping
   ↓
RPC Service
```

`rpcbind` helps clients determine which ports RPC services are using.

---

# Initial Nmap Scan

For Unix/Linux RPC:

```bash
nmap -sV -p 111 TARGET
```

For Microsoft RPC:

```bash
nmap -sV -p 135 TARGET
```

Broader Windows enumeration commonly includes:

```bash
nmap -sV -p 135,139,445 TARGET
```

---

# rpcinfo

On Unix-like systems, `rpcinfo` can query rpcbind.

Authorized/lab example:

```bash
rpcinfo -p TARGET
```

Potential output may identify services such as:

```text
portmapper
mountd
nfs
status
```

This is particularly useful when NFS is present.

---

# RPC + NFS

A common Unix enumeration relationship:

```text
111
 ↓
rpcbind
 ↓
NFS Services
 ↓
2049
```

If NFS is discovered, investigate it as a separate network service.

NFS deserves its own note later if you decide to expand the Enumeration module.

---

# Microsoft RPC

Windows uses RPC extensively for:

- Service management
- Domain operations
- Administrative functions
- Remote management
- Named-pipe communication

A Windows host exposing:

```text
135
139
445
```

should be considered for broader Windows/SMB enumeration.

---

# rpcclient

Samba provides:

```text
rpcclient
```

which can interact with Microsoft RPC interfaces.

Authorized example:

```bash
rpcclient -U USER TARGET
```

The server then requests authentication.

---

# Null / Anonymous Sessions

Historically, some Windows/Samba systems permitted useful RPC queries without authenticated credentials.

Modern systems typically restrict this heavily.

In intentionally vulnerable labs, anonymous access may still be available.

Do not assume null-session enumeration will work against modern Windows.

---

# rpcclient Help

After connecting:

```text
help
```

shows supported commands.

Depending on permissions, commands may query categories such as:

```text
Domain information
Users
Groups
Policies
```

The actual capabilities vary with target configuration and account privileges.

---

# SID

Windows security principals are identified using:

```text
SID
```

Security Identifier.

Conceptually:

```text
S-1-5-21-...
```

Users and groups receive unique SIDs.

---

# RID

A RID (Relative Identifier) is the final component of many Windows SIDs.

Conceptually:

```text
Domain SID
     +
RID
     ↓
Object SID
```

Understanding SIDs and RIDs becomes important during Windows and Active Directory enumeration.

---

# User Enumeration

Where permissions allow, RPC interfaces may provide information about domain/local accounts.

Modern environments increasingly restrict anonymous enumeration, so authenticated access is generally more useful.

---

# Group Enumeration

Groups are often more valuable than raw user lists.

Examples:

```text
Administrators
Remote Desktop Users
Backup Operators
Domain Admins
```

Group membership can reveal privilege relationships.

---

# Domain Information

RPC may expose clues such as:

```text
Domain name
Domain SID
Server information
Password policy
```

depending on the target and authentication level.

---

# enum4linux-ng

`enum4linux-ng` combines several SMB/RPC enumeration techniques.

Authorized/lab example:

```bash
enum4linux-ng TARGET
```

Potential information:

```text
Hostname
Workgroup/domain
Shares
Users
Groups
Policies
```

Not every query will succeed.

Always manually validate important findings.

---

# NetExec

NetExec can perform Windows/SMB enumeration using authenticated credentials where authorized.

Conceptually:

```bash
nxc smb TARGET -u USER -p PASSWORD
```

We'll cover NetExec more extensively in the Active Directory module.

---

# RPC + SMB

On Windows:

```text
SMB
 ↓
Named Pipes
 ↓
RPC Interfaces
```

This is why tools such as:

```text
rpcclient
enum4linux-ng
NetExec
Impacket
```

often appear together.

---

# RPC + Active Directory

During AD assessments:

```text
RPC
LDAP
SMB
Kerberos
DNS
```

all contribute different pieces of domain information.

Think of them as an ecosystem rather than independent protocols.

---

# Enumeration Workflow — Unix

```text
111 Open
   ↓
rpcbind
   ↓
rpcinfo
   ↓
Identify RPC Services
   ↓
NFS / Other Services
   ↓
Service-Specific Enumeration
```

---

# Enumeration Workflow — Windows

```text
135 / 445
     ↓
Windows RPC / SMB
     ↓
Authentication Context
     ↓
Host / Domain Information
     ↓
Users / Groups / Policies
     ↓
Correlate with LDAP + SMB
```

---

# Example Unix Notes

```text
Host:
10.10.10.70

Port:
111

Service:
rpcbind

RPC:
NFS-related services detected

Next:
Enumerate NFS
```

---

# Example Windows Notes

```text
Host:
10.10.10.80

Ports:
135
445

Environment:
Windows

Domain:
LAB

RPC:
Authenticated enumeration available

Interesting:
Domain groups identified
```

---

# Red Team Perspective

RPC is best treated as a **gateway to other information**.

Example:

```text
111
 ↓
rpcbind
 ↓
NFS
 ↓
Shared Files
```

or:

```text
445
 ↓
RPC
 ↓
Domain Information
 ↓
Users / Groups
 ↓
Active Directory Mapping
```

The value comes from understanding what RPC connects you to.

---

# Defensive Considerations

Administrators should:

- Restrict unnecessary RPC exposure
- Apply host/network firewalling
- Disable obsolete anonymous enumeration
- Limit administrative interfaces
- Keep Windows/Linux systems patched
- Monitor unusual remote RPC activity

---

# Common Beginner Mistakes

- Assuming RPC means one specific service.
- Confusing Unix rpcbind with Microsoft RPC.
- Ignoring UDP 111.
- Assuming null sessions work everywhere.
- Collecting usernames without examining groups.
- Ignoring NFS after finding rpcbind.
- Ignoring the relationship between RPC and SMB.
- Jumping into AD tooling without understanding RPC fundamentals.

---

# Interview Questions

### What is RPC?

A mechanism that allows a program to invoke procedures/services on another system.

### What commonly runs on port 111?

rpcbind/portmapper on Unix-like systems.

### What commonly uses port 135?

Microsoft RPC Endpoint Mapper.

### What is rpcclient?

A Samba utility for interacting with Microsoft RPC interfaces.

### What is a SID?

A Windows Security Identifier used to uniquely identify security principals.

### What is a RID?

The Relative Identifier component used within Windows SIDs.

### Why is RPC important for NFS?

rpcbind can advertise RPC services associated with NFS.

---

# Quick Revision

```text
Unix:

111 TCP/UDP
 ↓
rpcbind
 ↓
rpcinfo
 ↓
NFS / RPC Services

Windows:

135
445
 ↓
Microsoft RPC
 ↓
Users / Groups / Domain Info

Tools:

rpcinfo
rpcclient
enum4linux-ng
NetExec
Nmap
```

---

# Practice

- Metasploitable
- TryHackMe network-service labs
- Hack The Box
- HTB Academy
- Windows AD lab
- Linux NFS/RPC lab

---

# References

- RFC 5531 — ONC RPC
- Microsoft RPC Documentation
- Samba rpcclient Documentation
- Nmap Documentation
- NetExec Documentation
