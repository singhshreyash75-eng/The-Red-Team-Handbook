# Rubeus

> Kerberos-focused Active Directory security notes for authorized labs and assessments.

## What is Rubeus?

**Rubeus** is a Windows tool focused primarily on **Kerberos interaction and security testing** in Active Directory environments.

It is commonly associated with:

```text
Kerberos ticket enumeration
TGT requests
Service-ticket requests
Kerberoasting
AS-REP Roasting
Ticket inspection
Ticket import/export
Delegation testing
```

Think of:

```text
Rubeus
   ↓
Kerberos-focused AD tooling
```

---

# Kerberos Refresher

Normal Kerberos authentication:

```text
User
 |
 | AS-REQ
 v
KDC
 |
 | AS-REP
 | TGT
 v
User
 |
 | TGS-REQ
 v
KDC
 |
 | TGS-REP
 | Service Ticket
 v
Service
```

Important terms:

```text
KDC = Key Distribution Center
TGT = Ticket Granting Ticket
TGS = Ticket Granting Service
SPN = Service Principal Name
```

Rubeus interacts with several parts of this workflow.

---

# Why Rubeus is Useful

Native Windows provides:

```cmd
klist
```

for basic Kerberos-ticket inspection.

Rubeus provides additional Kerberos-focused functionality useful for security research and authorized testing.

Conceptually:

```text
Windows Kerberos
       |
       v
     Rubeus
       |
       +--> Inspect
       +--> Request
       +--> Analyze
       +--> Manage tickets
```

---

# Ticket Enumeration

Kerberos tickets currently available to an identity/system can reveal useful authentication context.

Native command:

```cmd
klist
```

Information can include:

```text
Client principal
Service principal
Domain/realm
Ticket lifetime
Encryption type
```

Rubeus can provide additional Kerberos-ticket visibility in appropriate security-testing contexts.

---

# TGT

A **Ticket Granting Ticket** is obtained during initial Kerberos authentication.

```text
Credentials
    ↓
   KDC
    ↓
   TGT
```

The TGT is then used to request service tickets.

---

# Service Tickets

When accessing a Kerberos-enabled service:

```text
TGT
 ↓
TGS request
 ↓
KDC
 ↓
Service Ticket
 ↓
Service
```

Examples of service classes:

```text
CIFS
HTTP
LDAP
MSSQLSvc
HOST
```

---

# SPNs

**SPN = Service Principal Name**

An SPN maps a Kerberos service to an account.

Example structure:

```text
MSSQLSvc/sql01.corp.local:1433
```

SPNs are especially important when studying:

```text
Kerberoasting
```

---

# Kerberoasting

Rubeus is commonly used in authorized labs to identify/request Kerberos service-ticket material associated with SPN accounts.

Conceptually:

```text
Domain User
     ↓
SPN Enumeration
     ↓
Service Ticket
     ↓
Offline password audit
```

The underlying security concern is usually:

```text
Weak service-account password
```

rather than a flaw in Kerberos itself.

---

# AS-REP Roasting

Rubeus also supports assessment of accounts configured without Kerberos preauthentication.

Condition:

```text
DONT_REQ_PREAUTH
```

Concept:

```text
User with preauth disabled
        ↓
AS-REQ
        ↓
AS-REP
        ↓
Password-derived encrypted material
        ↓
Offline password audit
```

---

# Ticket Reuse

Kerberos tickets are authentication artifacts.

If a valid ticket is exposed:

```text
Kerberos Ticket
       ↓
Ticket reuse
       ↓
Authentication as represented identity
```

This leads to the security concept:

```text
Pass-the-Ticket
```

Therefore Kerberos tickets should be protected like credentials.

---

# Ticket Import / Export

During Kerberos research, tickets may be represented in formats such as:

```text
.kirbi
.ccache
```

Common association:

```text
Windows tooling
    ↓
.kirbi

Linux Kerberos tooling
    ↓
.ccache
```

Exact formats/workflows depend on the toolset.

---

# Rubeus vs Mimikatz

Both can interact with Kerberos, but their focus differs.

```text
Rubeus
   |
Primarily Kerberos-focused


Mimikatz
   |
Broader Windows credential
and authentication research
```

---

# Rubeus vs Impacket

```text
Rubeus
   |
Windows
   |
Kerberos focused
```

versus:

```text
Impacket
   |
Python
   |
Broad Windows/network protocols
```

Impacket commonly operates from Linux assessment systems, while Rubeus is commonly encountered directly on Windows.

---

# Important Security Questions

When examining Kerberos activity, ask:

```text
Which user?
Which domain?
Which ticket?
Which SPN?
Which encryption type?
Which service?
How long is the ticket valid?
Why was the ticket requested?
```

---

# Detection

Defenders can monitor:

```text
Kerberos authentication events
TGT requests
Service-ticket requests
Unusual SPN activity
Abnormal ticket lifetimes
Unexpected encryption types
Unusual account/service combinations
```

Context matters because Kerberos-ticket activity is normal in AD.

---

# Defensive Mitigation

Important defenses include:

- Strong service-account credentials
- gMSAs where appropriate
- Kerberos preauthentication
- Least privilege
- Secure administrative tiering
- Protection of credential material
- Modern Kerberos encryption
- Monitoring Domain Controller authentication events

---

# Tool Relationship

```text
                 AD Security Tools
                       |
        +--------------+--------------+
        |              |              |
     Rubeus         Mimikatz       Impacket
        |              |              |
    Kerberos        Credentials     Protocols
     focused        + Kerberos      + Remote
```

---

# Key Takeaway

Think:

```text
Rubeus
   ↓
Kerberos
   ↓
TGT + Service Tickets + SPNs
   ↓
Authentication analysis
```

Understanding Kerberos is more important than memorizing individual Rubeus commands.
