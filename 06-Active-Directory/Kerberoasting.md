# Kerberoasting

> Kerberos service-account password auditing concepts for authorized Active Directory labs.

## What is Kerberoasting?

**Kerberoasting** is an Active Directory technique involving Kerberos service tickets associated with accounts that have **Service Principal Names (SPNs)**.

A domain-authenticated user can normally request service tickets for services they are permitted to discover/use.

Parts of those tickets are protected using keys associated with the service account.

This can enable **offline password guessing** against weak service-account passwords.

---

# Core Idea

```text
Domain User
    |
    | identify SPN
    v
Service Account
    |
    | request service ticket
    v
KDC
    |
    | ticket protected with
    | service-account key
    v
Service Ticket
    |
    v
Offline password analysis
```

---

# SPN

**SPN = Service Principal Name**

An SPN uniquely identifies a service instance for Kerberos.

Examples of service classes:

```text
MSSQLSvc
HTTP
CIFS
LDAP
HOST
```

Conceptually:

```text
MSSQLSvc/sql01.corp.local:1433
```

This tells Kerberos:

```text
Service class
     +
Host
     +
Optional port/instance
```

---

# Service Accounts

Services often run using dedicated accounts.

Example:

```text
CORP\sqlservice
```

The account may have an SPN such as:

```text
MSSQLSvc/sql01.corp.local
```

Kerberos needs the account's key to protect the relevant service-ticket data.

---

# Kerberoasting Flow

Simplified:

```text
1. Authenticate to domain

          ↓

2. Enumerate accounts with SPNs

          ↓

3. Request service ticket

          ↓

4. KDC returns ticket

          ↓

5. Extract password-verification material

          ↓

6. Perform offline password audit
```

---

# Why Does This Work?

The service ticket contains data encrypted/protected using a key derived from the service account's credential.

Therefore:

```text
Service Ticket
      +
Candidate password
      ↓
Offline verification
```

If the service account uses a weak password, password guessing may eventually recover it.

---

# Important Requirement

Unlike AS-REP Roasting, Kerberoasting does **not** depend on:

```text
Do not require Kerberos preauthentication
```

Instead, the important relationship is:

```text
Account
   +
SPN
```

---

# Typical Prerequisite

Usually an attacker/tester already has:

```text
Valid domain authentication
```

A standard domain account may be sufficient to request many service tickets.

Therefore:

```text
Low-privileged domain access
              +
Weak service-account password
              =
Potential privilege path
```

---

# Common Tools

Authorized labs commonly demonstrate Kerberoasting with:

```text
Impacket
Rubeus
PowerView
Native AD/PowerShell enumeration
```

Common Impacket utility:

```text
GetUserSPNs.py
```

Rubeus also provides Kerberos service-ticket auditing functionality.

---

# Enumeration

The first goal is to identify:

```text
Which accounts have SPNs?
```

With Microsoft's ActiveDirectory PowerShell module, SPN-bearing accounts can be identified through directory attributes.

Conceptually:

```text
Users
  |
Filter
  |
servicePrincipalName exists
  |
  v
Service Accounts
```

---

# What Makes a Target Interesting?

Not every SPN account is equally important.

Prioritize security review based on:

```text
Password strength
Password age
Privileges
Group memberships
Service importance
Account type
Encryption configuration
```

Example:

```text
sqlservice
    |
SPN configured
    |
Weak old password
    |
Privileged group
```

This is much more significant than a strongly protected, minimally privileged service identity.

---

# Offline Password Guessing

The important security characteristic is:

```text
Ticket obtained from KDC
        ↓
Password testing happens offline
```

This means repeated password guesses do not necessarily generate repeated interactive logon attempts.

However, the initial Kerberos ticket requests themselves can still be logged and monitored.

---

# Encryption Types

Kerberos can use different encryption types depending on AD/account configuration and platform capabilities.

You may encounter:

```text
RC4
AES128
AES256
```

Encryption configuration can affect password-auditing economics and detection context.

Strong, randomly generated service-account passwords remain critical regardless.

---

# Kerberoasting vs AS-REP Roasting

## Kerberoasting

Targets:

```text
SPN-associated accounts
```

Material:

```text
Service ticket
```

Usually requires:

```text
Domain authentication
```

---

## AS-REP Roasting

Targets:

```text
Accounts with preauthentication disabled
```

Material:

```text
AS-REP
```

Configuration:

```text
DONT_REQ_PREAUTH
```

---

# Comparison

```text
             Kerberoasting          AS-REP Roasting
             -------------          ---------------

Target       SPN account            Preauth-disabled user

Kerberos     Service ticket         AS-REP
material

Key issue    Weak service           Weak user password
             password               + bad configuration

Main AD      SPN                     DONT_REQ_PREAUTH
condition
```

---

# Kerberoasting vs Golden Ticket

These occur at very different stages.

```text
Kerberoasting
     |
Potential credential acquisition
     |
Targets service-account password


Golden Ticket
     |
Post-domain compromise
     |
Requires KRBTGT key material
```

Do not treat them as equivalent attacks.

---

# Managed Service Accounts

One defensive improvement is using managed service-account technologies such as:

```text
gMSA
=
Group Managed Service Account
```

gMSAs can provide automatically managed, high-entropy credentials.

Conceptually:

```text
Traditional service account
        |
Human-managed password
        |
May become old/weak


gMSA
        |
Automatically managed credential
        |
High entropy / rotation
```

This can substantially reduce password-guessing risk when correctly deployed.

---

# Defensive Mitigation

Protect service accounts with:

- Long, random passwords
- gMSAs where appropriate
- Least privilege
- Removal of unnecessary SPNs
- Regular service-account review
- Appropriate Kerberos encryption configuration
- Monitoring unusual service-ticket requests

Most importantly:

```text
Do not use human-style passwords
for service accounts.
```

---

# Detection

Defenders can analyze Kerberos service-ticket activity on Domain Controllers.

Look for patterns such as:

```text
Large numbers of service-ticket requests
Unusual users requesting many SPNs
Requests for rarely accessed services
Suspicious encryption patterns
Subsequent abnormal authentication
```

Context is essential because service-ticket requests are normal Kerberos behavior.

---

# Security Assessment Workflow

```text
Enumerate SPNs
      ↓
Identify service accounts
      ↓
Review privileges
      ↓
Request permitted Kerberos ticket data
      ↓
Assess password strength offline
      ↓
Validate impact
```

The objective of an authorized password audit is not simply:

```text
"Can I obtain a ticket?"
```

The meaningful question is:

```text
"Does weak credential management
turn this normal Kerberos functionality
into a privilege path?"
```

---

# Key Takeaway

Remember:

```text
Service Account
      +
     SPN
      ↓
Service Ticket
      ↓
Password-derived protection
      ↓
Offline password audit
```

Kerberoasting does **not** exploit a broken Kerberos protocol.

The primary weakness is typically:

```text
Normal Kerberos functionality
          +
Weak service-account credential
          +
Potentially excessive privileges
```
