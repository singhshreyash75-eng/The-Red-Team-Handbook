# 🪟 SMB Enumeration

> Server Message Block (SMB) is a network protocol used primarily for file sharing, printer sharing, and inter-process communication. SMB is especially important in Windows and Active Directory environments.

---

## Overview

SMB commonly appears on:

```text
445/tcp
```

Older systems may also use:

```text
139/tcp
```

During Windows enumeration, SMB can reveal significant information about:

- Hosts
- Shares
- Files
- Users
- Domain/workgroup information
- Access permissions

---

## Why SMB Matters

SMB is deeply integrated into Windows networks.

You will encounter it again in:

```text
Windows Enumeration
        ↓
Active Directory
        ↓
Credentialed Enumeration
        ↓
Lateral Movement
```

Understanding basic SMB enumeration now makes the AD module much easier later.

---

## SMB Architecture

Simplified:

```text
Client
   │
   │ SMB
   ▼
Windows Server
   │
   ├── Shared Files
   ├── Printers
   ├── Named Pipes
   └── Administrative Services
```

---

## Common Ports

| Port | Service |
|---:|---|
| 139/TCP | NetBIOS Session Service / legacy SMB transport |
| 445/TCP | SMB directly over TCP |

Modern Windows environments primarily use TCP 445.

---

## SMB Versions

Important generations include:

```text
SMBv1
SMBv2
SMBv3
```

SMBv1 is legacy and should generally be disabled.

Modern Windows versions use newer SMB implementations with significantly improved security.

---

## Initial Nmap Enumeration

Authorized/lab example:

```bash
nmap -p 139,445 -sV TARGET
```

Default scripts:

```bash
nmap -sC -sV -p 139,445 TARGET
```

Potential information:

```text
Hostname
Workgroup
SMB Version
Operating System clues
```

---

## SMB NSE Scripts

List installed scripts:

```bash
ls /usr/share/nmap/scripts/smb*
```

Common enumeration scripts may include:

```text
smb-os-discovery
smb-enum-shares
smb-enum-users
smb-protocols
```

Availability varies with Nmap versions.

Read script documentation before execution.

---

## SMB Protocol Enumeration

Example:

```bash
nmap --script smb-protocols -p 445 TARGET
```

This can help determine supported SMB protocol versions.

---

## SMB OS Discovery

Example:

```bash
nmap --script smb-os-discovery -p 445 TARGET
```

Depending on server configuration, information may include:

```text
Computer Name
Domain
Workgroup
OS clues
```

---

## SMB Shares

A **share** is a network-accessible resource.

Examples:

```text
Public
Users
Documents
Backups
Development
```

Windows also uses administrative shares such as:

```text
C$
ADMIN$
IPC$
```

Access depends on authentication and permissions.

---

## smbclient

`smbclient` provides an FTP-like interface for SMB.

List shares:

```bash
smbclient -L //TARGET/
```

If anonymous/guest access is intentionally enabled in a lab:

```bash
smbclient -L //TARGET/ -N
```

`-N` avoids prompting for a password.

Whether this succeeds depends entirely on server configuration.

---

## Connecting to a Share

Authorized example:

```bash
smbclient //TARGET/SHARE -U username
```

Inside the client, useful commands include:

```text
ls
cd
pwd
get
```

Use:

```text
help
```

to see available commands.

---

## Anonymous SMB

Some systems intentionally or accidentally permit guest/anonymous access.

Check only where authorized.

Potential findings may include:

```text
Public documents
Configuration backups
Installation files
Documentation
```

Anonymous access itself is not automatically a vulnerability; impact depends on what is exposed and intended policy.

---

## rpcclient

`rpcclient` can interact with Microsoft RPC services exposed through SMB.

Authorized/lab example:

```bash
rpcclient -U username TARGET
```

In environments permitting anonymous access, a null/empty credential attempt may reveal limited information.

Whether this works depends on modern Windows security policy and server configuration.

RPC enumeration is covered separately in `RPC.md`.

---

## enum4linux-ng

`enum4linux-ng` automates several SMB/RPC enumeration tasks.

Typical authorized/lab usage:

```bash
enum4linux-ng TARGET
```

It may attempt to collect:

- Host information
- Shares
- Domain/workgroup information
- Users, where exposed

Treat automated output as leads that should be manually verified.

---

## NetExec

Modern Windows/AD assessments commonly use **NetExec** for protocol enumeration.

Basic authenticated SMB syntax conceptually follows:

```bash
nxc smb TARGET -u USER -p PASSWORD
```

NetExec evolves quickly; consult its current official documentation before using modules or advanced flags.

We'll cover it more extensively in the Active Directory module.

---

## Share Enumeration Workflow

```text
445 Open
   ↓
Identify SMB Version
   ↓
Determine Authentication Requirements
   ↓
Enumerate Accessible Shares
   ↓
Inspect Authorized Content
   ↓
Record Permissions
```

---

## Share Permissions

Different users may have:

```text
Read
Write
Full Control
No Access
```

Writable shares can be security-sensitive, particularly if other systems or privileged users consume files from them.

Do not assume write access alone equals code execution.

Understand how the share is actually used.

---

## Interesting File Types

During an authorized assessment, exposed shares may contain:

```text
.txt
.ini
.xml
.config
.ps1
.bat
.cmd
.xlsx
.docx
.zip
.bak
```

Potentially useful information includes:

- Configuration
- Internal documentation
- Hostnames
- Usernames
- Deployment scripts

Handle sensitive data according to the engagement rules.

---

## SMB Signing

SMB signing helps protect SMB communications against tampering.

Whether signing is:

```text
Required
Enabled but not required
Disabled
```

can matter in certain Windows attack scenarios.

We'll revisit this in the Active Directory module.

---

## SMBv1

Legacy SMBv1 has historically been associated with severe vulnerabilities.

A famous example:

```text
MS17-010
```

which affected SMBv1 implementations and was associated with the EternalBlue exploit family.

However:

```text
SMBv1 enabled
```

does not automatically prove a specific vulnerability.

Patch state and system configuration still matter.

---

## Version → CVE Workflow

Suppose SMB enumeration identifies an old Windows system.

Do:

```text
OS / SMB Information
        ↓
Vendor Security Advisories
        ↓
Patch Context
        ↓
NVD / CISA KEV
        ↓
Controlled Validation
```

Do not blindly launch an exploit based only on a scanner banner.

---

## Common Tools

```text
Nmap
smbclient
rpcclient
enum4linux-ng
NetExec
Impacket
```

Impacket becomes particularly important later during Active Directory testing.

---

## Enumeration Checklist

When you find `445/tcp`:

```text
□ SMB version
□ Hostname
□ Domain / workgroup
□ Signing configuration
□ Authentication requirements
□ Accessible shares
□ Share permissions
□ Interesting files
□ OS clues
```

---

## Example Notes

```text
Host:
10.10.10.20

Port:
445

Protocol:
SMB

Hostname:
FILE01

Domain:
LAB

Shares:
Public
Users

Anonymous:
Public readable

Interesting:
Public contains deployment documentation
```

---

## Red Team Perspective

SMB is not simply:

```text
445 → EternalBlue
```

That is a beginner mistake.

A better mindset:

```text
445
 ↓
SMB Version
 ↓
Domain / Host Information
 ↓
Authentication
 ↓
Shares
 ↓
Permissions
 ↓
Files
 ↓
Relationships
```

In real Windows networks, information exposed through SMB can be more valuable than a direct exploit.

---

## Common Beginner Mistakes

- Seeing port 445 and immediately trying EternalBlue.
- Assuming anonymous access always works.
- Ignoring share permissions.
- Ignoring SMB signing.
- Not documenting domain/workgroup information.
- Treating scanner output as definitive.
- Downloading unnecessary sensitive files.
- Forgetting that modern Windows systems differ significantly from old CTF boxes.

---

## Interview Questions

### What is SMB?

A protocol used for file sharing, printer sharing, and other Windows network communication.

### Which port does modern SMB commonly use?

TCP 445.

### What is a share?

A network-accessible resource exposed through SMB.

### What is SMB signing?

A mechanism that provides integrity/authentication protections for SMB messages.

### Why is SMB important in Active Directory?

Windows domains rely heavily on SMB for file access and several administrative workflows.

### What was MS17-010?

A Microsoft security bulletin covering serious SMBv1 vulnerabilities affecting older Windows systems.

---

## Quick Revision

```text
Ports:
139
445

Tools:
nmap
smbclient
rpcclient
enum4linux-ng
NetExec
Impacket

Workflow:

445
 ↓
Version
 ↓
Hostname / Domain
 ↓
Authentication
 ↓
Shares
 ↓
Permissions
 ↓
Files

Remember:

445 ≠ Automatically EternalBlue
```

---

## Practice

- TryHackMe Windows/SMB rooms
- Hack The Box Academy
- Metasploitable
- Windows lab domain
- Authorized HTB machines

---

## References

- Microsoft SMB Documentation
- Nmap NSE Documentation
- Samba Documentation
- NetExec Documentation
- Impacket Documentation
- Microsoft Security Guidance
