# Pass-the-Hash

> NTLM Pass-the-Hash fundamentals for authorized Active Directory labs and defensive analysis.

## What is Pass-the-Hash?

**Pass-the-Hash (PtH)** is an authentication technique where an NTLM-compatible password hash is used as authentication material without first recovering the original plaintext password.

Conceptually:

```text
Password
   ↓
NT Hash
   ↓
Authentication
```

Normally a user knows:

```text
Password
```

With Pass-the-Hash, the relevant situation becomes:

```text
NT Hash
   ↓
Authentication mechanism
   ↓
Access as that account
```

---

# Why is this Important?

Suppose an attacker obtains:

```text
Username
+
NT hash
```

They may not need to crack:

```text
NT hash
   ↓
Plaintext password
```

before certain NTLM-based authentication workflows become security-relevant.

Therefore:

```text
Hash == Sensitive Credential Material
```

Treat password hashes like credentials.

---

# NT Hash

Windows passwords are associated with an NT hash.

Conceptually:

```text
Password
   ↓
Hash function
   ↓
NT Hash
```

The hash is not simply the plaintext password.

However, possession of the hash can still be dangerous because some authentication mechanisms rely on password-derived secrets.

---

# NTLM Authentication

Simplified NTLM challenge-response:

```text
Client                         Server
  |                              |
  |-------- Request ------------>|
  |                              |
  |<------- Challenge -----------|
  |                              |
  |-------- Response ------------>|
  |                              |
  |       Verification           |
```

The password itself is not normally sent directly across the network.

Instead, password-derived material participates in producing the response.

---

# Pass-the-Hash Concept

Traditional:

```text
Know password
     ↓
Derive credential material
     ↓
Authenticate
```

PtH:

```text
Already possess hash
       ↓
Use hash-derived material
       ↓
Authenticate
```

Therefore password cracking is not always required before stolen hashes become useful.

---

# Prerequisites

For PtH to have meaningful impact, several conditions generally need to align:

```text
Valid NT hash
      +
Valid account
      +
Target supports relevant authentication
      +
Account has access to target
```

Possessing a hash does not automatically provide administrative access everywhere.

---

# Authorization Still Matters

Important:

```text
Authentication
      !=
Authorization
```

If:

```text
Alice's hash
```

is available but Alice has no administrative access to `SERVER01`, using Alice's authentication material does not magically make her administrator.

Conceptually:

```text
Hash
  ↓
Authenticate as Alice
  ↓
Alice's actual permissions
```

---

# Local Accounts

Local administrator credentials are particularly important when the same local password is reused across multiple systems.

Example:

```text
PC01
Administrator -> Same password

PC02
Administrator -> Same password

PC03
Administrator -> Same password
```

If credential material is exposed on one machine:

```text
PC01 compromise
      ↓
Local admin hash
      ↓
Same credential elsewhere
      ↓
Potential lateral movement
```

This is why local administrator password reuse is dangerous.

---

# Windows LAPS

A major defense against local administrator credential reuse is:

```text
Windows LAPS
```

LAPS manages unique local administrator passwords.

Instead of:

```text
100 machines
     ↓
Same local admin password
```

use:

```text
PC01 -> unique password
PC02 -> unique password
PC03 -> unique password
```

This significantly reduces credential-reuse paths.

---

# Domain Accounts

Domain-account hashes can also be highly sensitive.

Impact depends on:

```text
Account privileges
Target permissions
Authentication configuration
Network accessibility
Security controls
```

Example:

```text
Domain User hash
      ↓
Limited permissions
```

versus:

```text
Privileged Admin hash
      ↓
Administrative access
      ↓
High impact
```

---

# Common Tools Associated With PtH

In authorized security labs you may encounter:

```text
Mimikatz
Impacket
NetExec / CrackMapExec
Windows administration tooling
```

The important concept is not the tool name.

It is:

```text
Authentication material
        +
Protocol
        +
Authorization
```

---

# Pass-the-Hash vs Password Cracking

These are different.

## Password Cracking

```text
Hash
 ↓
Guess passwords
 ↓
Recover plaintext
```

## Pass-the-Hash

```text
Hash
 ↓
Use directly as authentication material
```

Therefore:

```text
PtH does not inherently require
recovering the plaintext password.
```

---

# Pass-the-Hash vs Pass-the-Ticket

Do not confuse them.

## Pass-the-Hash

Uses:

```text
NT hash / NTLM credential material
```

Typically associated with:

```text
NTLM
```

## Pass-the-Ticket

Uses:

```text
Kerberos ticket
```

Associated with:

```text
Kerberos
```

Comparison:

```text
Pass-the-Hash
      |
   NT Hash
      |
    NTLM


Pass-the-Ticket
      |
Kerberos Ticket
      |
  Kerberos
```

---

# Pass-the-Hash vs Kerberoasting

Also different.

```text
Kerberoasting
      ↓
Obtain service-ticket material
      ↓
Offline password guessing
```

versus:

```text
Pass-the-Hash
      ↓
Already possess NT hash
      ↓
Use authentication material
```

---

# Credential Reuse

One of the most important concepts behind lateral movement is:

```text
Credential A
     |
works on
     |
System 1
     +
System 2
     +
System 3
```

Credential reuse turns:

```text
One compromised machine
```

into potentially:

```text
Many compromised machines
```

---

# Lateral Movement

Conceptually:

```text
Initial Host
    |
Credential Material
    |
    v
Second Host
    |
Additional Credentials
    |
    v
Third Host
```

This is known as:

```text
Lateral Movement
```

PtH can be one mechanism enabling such movement when authorization and protocol conditions permit it.

---

# Detection

Defenders should monitor:

```text
NTLM authentication
Unexpected administrative logons
Remote service activity
SMB authentication
Credential-dumping behavior
Unusual account-to-host relationships
```

Important Windows security events and endpoint telemetry can help identify suspicious authentication patterns.

---

# Defensive Mitigation

### 1. Deploy LAPS

```text
Unique local admin passwords
```

### 2. Minimize NTLM

Where operationally possible:

```text
Prefer Kerberos
Reduce unnecessary NTLM usage
```

### 3. Credential Guard

Protect sensitive authentication material.

### 4. Administrative Tiering

Avoid:

```text
Domain Admin
    ↓
Normal workstation
```

Prefer separated administrative environments.

### 5. Least Privilege

Reduce the number of accounts with administrative access.

### 6. Restrict Lateral Administration

Limit:

```text
SMB
RPC
WMI
Remote administration
```

between systems where unnecessary.

---

# Key Takeaway

Remember:

```text
Password
   ↓
NT Hash
```

The dangerous assumption is:

```text
"If the attacker doesn't know
the plaintext password,
the credential is safe."
```

That is false in environments where hash-derived material can satisfy an authentication workflow.

Therefore:

```text
NT Hash
   =
Credential Material
```

Protect hashes with the same seriousness as passwords.
