# Mimikatz

> Windows credential and Kerberos security concepts for authorized Active Directory labs and defensive analysis.

## What is Mimikatz?

**Mimikatz** is a Windows security research tool created by Benjamin Delpy.

It is widely known for demonstrating how Windows authentication material can be exposed after an attacker obtains sufficient access to a system.

Mimikatz is commonly associated with:

```text
Windows credentials
NTLM hashes
Kerberos tickets
LSASS
Credential material
Golden Tickets
Silver Tickets
Pass-the-Hash
Pass-the-Ticket
```

Important:

```text
Mimikatz != Initial Access
```

In most scenarios, useful Mimikatz operations require an attacker/tester to already have significant access to the machine.

---

# Core Security Concept

Windows must maintain authentication information so users and services can authenticate to resources.

Conceptually:

```text
User logs in
     ↓
Windows authentication
     ↓
Authentication material exists
     ↓
Processes/services use it
```

If a highly privileged process can access sensitive authentication material, credential exposure becomes possible.

---

# LSASS

One of the most important Windows processes when studying credentials is:

```text
lsass.exe
```

**LSASS = Local Security Authority Subsystem Service**

LSASS participates in:

```text
User authentication
Security policy
Access tokens
Credential handling
Kerberos
NTLM
```

Conceptually:

```text
        LSASS
          |
    +-----+-----+
    |           |
 Kerberos      NTLM
    |           |
 Tickets      Credential
              material
```

Because LSASS handles sensitive authentication information, access to it is highly security-sensitive.

---

# Privilege Requirements

Credential-access operations normally require elevated privileges.

Useful Windows enumeration:

```cmd
whoami
whoami /groups
whoami /priv
```

Check whether the current context is:

```text
Standard User
Administrator
SYSTEM
```

These contexts have very different security capabilities.

---

# Administrator vs SYSTEM

Windows has highly privileged local security contexts.

Conceptually:

```text
Standard User
      ↓
Administrator
      ↓
SYSTEM
```

`NT AUTHORITY\SYSTEM` is a highly privileged local identity used by Windows services and operating-system components.

Check current identity:

```cmd
whoami
```

---

# Credential Material

Mimikatz is often discussed in relation to several credential forms.

Keep them separate:

```text
Plaintext password
NTLM hash
Kerberos TGT
Kerberos service ticket
Credential keys
Access tokens
```

They are not the same thing.

---

# NTLM Hashes

Windows commonly stores/uses password-derived NT hash material.

Conceptually:

```text
Password
   ↓
NT hash
   ↓
NTLM authentication workflows
```

An NT hash is security-sensitive because some Windows authentication mechanisms can use hash-derived material without requiring recovery of the original plaintext password.

This leads to concepts such as:

```text
Pass-the-Hash
```

---

# Kerberos Tickets

Mimikatz can also interact with Kerberos authentication artifacts.

Important ticket types:

```text
TGT
Service Ticket
```

Normal Kerberos:

```text
User
 ↓
KDC
 ↓
TGT
 ↓
Service Ticket
 ↓
Service
```

Ticket abuse concepts include:

```text
Pass-the-Ticket
Golden Ticket
Silver Ticket
```

---

# Pass-the-Hash

Conceptually:

```text
NT hash obtained
      ↓
Authentication protocol accepts
hash-derived credential material
      ↓
Authenticate without knowing
plaintext password
```

This is covered separately in:

```text
Pass-the-Hash.md
```

---

# Pass-the-Ticket

Instead of password/hash material, an attacker may obtain a valid Kerberos ticket.

Conceptually:

```text
Kerberos Ticket
      ↓
Reuse ticket
      ↓
Authenticate as ticket identity
```

This is known as:

```text
Pass-the-Ticket
```

---

# Golden Ticket

Mimikatz is historically associated with Golden Ticket research.

Golden Ticket:

```text
KRBTGT secret
      ↓
Forge TGT
      ↓
Kerberos identity impersonation
```

Required prerequisite:

```text
KRBTGT key material
```

This generally implies severe prior domain compromise.

---

# Silver Ticket

Silver Tickets involve service-account/computer key material rather than KRBTGT.

```text
Service key
    ↓
Forge service ticket
    ↓
Specific service
```

Compare:

```text
Golden Ticket
    |
KRBTGT
    |
Forged TGT


Silver Ticket
    |
Service account key
    |
Forged service ticket
```

---

# Common Mimikatz Terminology

When reading labs or reports, you may encounter terms such as:

```text
sekurlsa
kerberos
lsadump
token
crypto
privilege
```

These represent different functionality areas.

For learning, understand what Windows component each category interacts with rather than memorizing commands.

---

# Credential Exposure Chain

A common conceptual attack path is:

```text
Compromise workstation
        ↓
Gain administrative privileges
        ↓
Access credential material
        ↓
Discover privileged credential/session
        ↓
Authenticate to another system
        ↓
Lateral movement
```

This explains why privileged users should avoid logging into lower-trust systems.

---

# Credential Tiering

Suppose:

```text
Domain Admin
     |
logs into
     v
Workstation
```

If that workstation is compromised:

```text
Compromised Workstation
        ↓
Potential credential exposure
        ↓
Domain Admin identity
        ↓
Domain compromise
```

Therefore administrative tiering is important.

---

# Credential Guard

Windows provides security mechanisms designed to reduce credential theft.

One important technology is:

```text
Windows Defender Credential Guard
```

It uses virtualization-based security to isolate certain secrets.

Other protections include:

```text
LSASS protection
Credential Guard
VBS
Restricted Admin
Remote Credential Guard
Administrative tiering
```

---

# LSASS Protection

Windows can protect LSASS using mechanisms such as:

```text
RunAsPPL
```

PPL means:

```text
Protected Process Light
```

This makes unauthorized interaction with LSASS more difficult.

---

# Detection

Defenders should monitor for behavior involving:

```text
Suspicious LSASS access
Credential dumping
Unexpected process handles
Security-tool tampering
Unusual Kerberos tickets
NTLM authentication anomalies
Privileged account misuse
```

EDR products commonly monitor LSASS-related behavior closely.

---

# Defensive Mitigation

Reduce credential-exposure risk through:

- Credential Guard
- LSASS protection
- Least privilege
- Administrative tiering
- Separate admin accounts
- Strong endpoint security
- Restricting debug privileges
- Limiting privileged interactive logons
- EDR monitoring
- Regular credential rotation

Most importantly:

```text
Do not expose high-value credentials
to low-trust machines.
```

---

# Mimikatz vs Impacket

```text
Mimikatz
    |
Windows credential /
Kerberos internals


Impacket
    |
Network protocols /
remote Windows interaction
```

They frequently appear together in AD labs but solve different problems.

---

# Mimikatz vs Rubeus

```text
Mimikatz
    |
Broad credential +
Kerberos functionality


Rubeus
    |
Primarily Kerberos-focused
```

---

# Key Takeaway

Think of Mimikatz as a tool for understanding:

```text
Windows Authentication
         ↓
Credential Material
         ↓
LSASS / Kerberos / NTLM
         ↓
Credential Exposure
         ↓
Identity Abuse
```

The main defensive lesson is:

> Compromising one machine becomes significantly more dangerous when privileged credentials or sessions are exposed on that machine.
