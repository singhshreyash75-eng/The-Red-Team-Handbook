# Impacket

> Impacket fundamentals and common Active Directory use cases for authorized labs and administration.

## What is Impacket?

**Impacket** is a Python-based collection of implementations and tools for working with network protocols.

It is heavily used in Windows and Active Directory security labs.

Protocols/features commonly encountered include:

```text
SMB
MSRPC
NTLM
Kerberos
LDAP-related workflows
MSSQL
WMI
DCOM
```

Think of Impacket as:

```text
Python protocol libraries
          +
Ready-made utilities
```

---

# Why Impacket Matters in AD

Windows environments rely heavily on protocols such as:

```text
SMB
RPC
Kerberos
NTLM
LDAP
```

Impacket provides tooling that understands these protocols directly.

Therefore it is commonly used for:

```text
Enumeration
Authentication testing
Kerberos analysis
SMB interaction
Remote administration in labs
Credential-security assessment
```

---

# Common Impacket Utilities

Important names to recognize:

```text
GetNPUsers.py
GetUserSPNs.py
secretsdump.py
psexec.py
wmiexec.py
smbexec.py
atexec.py
smbclient.py
lookupsid.py
GetADUsers.py
Get-GPPPassword.py
ticketer.py
```

Tool availability/naming can vary with the installed Impacket version.

---

# GetNPUsers

Associated with:

```text
AS-REP Roasting
```

Conceptually:

```text
Domain users
      ↓
Identify accounts without preauthentication
      ↓
Request AS-REP material
```

Security condition:

```text
DONT_REQ_PREAUTH
```

---

# GetUserSPNs

Associated with:

```text
Kerberoasting
```

Conceptually:

```text
Domain account
      ↓
Enumerate SPNs
      ↓
Identify service accounts
      ↓
Request service-ticket material
```

Remember:

```text
GetNPUsers
    ↓
AS-REP Roasting

GetUserSPNs
    ↓
Kerberoasting
```

---

# SMBClient

Impacket includes SMB client functionality useful for authorized share administration/testing.

Conceptually:

```text
Client
  |
 SMB
  v
Server
  |
  +-- Share A
  +-- Share B
```

Before accessing data, determine:

```text
Which shares exist?
Which identity is being used?
What permissions does that identity have?
```

---

# lookupsid

SID/RID information is important in Windows environments.

Conceptually:

```text
SID
 |
 +--> User
 +--> Group
 +--> Computer
```

SID-related enumeration can help map principals when performing authorized domain analysis.

---

# secretsdump

`secretsdump.py` is associated with extracting credential-related secrets from Windows/AD sources when sufficient privileges are already available.

Potential sources can include:

```text
SAM
LSA secrets
Domain credential database material
```

This is a **post-compromise credential-access capability** and should only be used on systems where explicit authorization covers credential extraction.

Conceptually:

```text
Privileged access
       ↓
Credential store
       ↓
Credential material
```

---

# Remote Execution Utilities

Impacket includes multiple remote-administration/execution mechanisms.

Names commonly encountered:

```text
psexec.py
wmiexec.py
smbexec.py
atexec.py
```

They use different Windows mechanisms.

Conceptually:

```text
Authenticated administrative access
              ↓
Windows management mechanism
              ↓
Remote command execution
```

The important prerequisite is normally:

```text
Sufficient remote administrative authorization
```

---

# Kerberos Support

Impacket supports Kerberos-aware authentication workflows.

This matters because AD assessments should distinguish:

```text
Password authentication
NTLM/hash-based authentication
Kerberos ticket authentication
```

These are different authentication materials.

---

# Credential Formats

During authorized labs you may encounter syntax representing:

```text
domain
username
password
NT hash
Kerberos credential cache
```

Do not confuse:

```text
Password
   !=
NT hash
   !=
Kerberos ticket
```

They are different authentication artifacts.

---

# Kerberos Credential Cache

On Linux, Kerberos tickets are often represented using a credential cache:

```text
.ccache
```

Environment variable commonly involved:

```bash
KRB5CCNAME
```

Conceptually:

```text
Kerberos ticket
      ↓
Credential cache
      ↓
Kerberos-aware application
```

---

# Authentication Troubleshooting

Kerberos operations depend heavily on:

```text
DNS
Hostname resolution
Domain names
Time synchronization
SPNs
```

Useful checks:

```bash
date
cat /etc/resolv.conf
```

Windows:

```cmd
ipconfig /all
w32tm /query /status
```

A failed Kerberos operation does not automatically mean credentials are wrong.

---

# Impacket Workflow

Good methodology:

```text
1. Understand target service
2. Identify authentication method
3. Verify authorization
4. Choose protocol-specific tool
5. Collect minimum required information
6. Validate results manually
```

Do not approach Impacket as:

```text
"Run every tool"
```

Instead:

```text
Protocol
   ↓
Objective
   ↓
Appropriate Impacket utility
```

---

# Example Tool Mapping

```text
AS-REP analysis
      ↓
GetNPUsers

SPN/Kerberos service-account analysis
      ↓
GetUserSPNs

SMB interaction
      ↓
smbclient

SID enumeration
      ↓
lookupsid

Credential-store assessment
      ↓
secretsdump

Authorized remote administration
      ↓
psexec / wmiexec / smbexec / atexec
```

---

# Impacket vs Mimikatz

They overlap in some AD security workflows but are fundamentally different.

```text
Impacket
   |
Python/network protocols
   |
Often used from Linux


Mimikatz
   |
Windows credential/Kerberos internals
   |
Typically Windows-focused
```

---

# Impacket vs Rubeus

```text
Impacket
   |
Broad Windows networking/protocol toolkit

Rubeus
   |
Kerberos-focused Windows toolkit
```

---

# Defensive Perspective

Impacket activity may generate telemetry across:

```text
SMB
RPC
Kerberos
Authentication
Service creation
WMI
Scheduled tasks
```

Detection should focus on behavior rather than assuming a particular executable name will always appear.

---

# Key Takeaway

Think of Impacket as:

```text
          IMPACKET
              |
    +---------+---------+
    |         |         |
   SMB     Kerberos    RPC
    |         |         |
    +---------+---------+
              |
       Windows / AD
       interaction
```

Do not memorize every utility first.

Learn:

```text
Protocol
   +
Authentication
   +
Windows mechanism
```

Then the individual Impacket tools become much easier to understand.
