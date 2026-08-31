# AS-REP Roasting

> Active Directory Kerberos attack concept for authorized labs and security assessments.

## What is AS-REP Roasting?

**AS-REP Roasting** targets Active Directory accounts that have:

```text
"Do not require Kerberos preauthentication"
```

enabled.

Normally Kerberos requires the user to prove knowledge of their password-derived key **before** the KDC returns useful authentication material.

If preauthentication is disabled, this protection is removed.

---

## Normal Kerberos Preauthentication

Simplified flow:

```text
User
 |
 | AS-REQ + preauthentication data
 v
KDC
 |
 | verify user
 v
AS-REP
 |
 v
TGT
```

The client proves knowledge of its secret before receiving the AS-REP.

---

## Without Preauthentication

For an affected account:

```text
Attacker / Client
       |
       | AS-REQ
       v
      KDC
       |
       | AS-REP
       v
Encrypted material
```

The KDC can return an AS-REP without first requiring the normal preauthentication proof.

Part of that response is encrypted using material derived from the user's password.

This enables an **offline password-guessing attack**.

---

## Why is it Dangerous?

The important distinction is:

```text
Online guessing
     vs
Offline cracking
```

Once the relevant AS-REP material has been obtained:

```text
KDC interaction
      ↓
AS-REP material
      ↓
Offline password guessing
      ↓
No repeated authentication attempts against DC
```

Therefore weak passwords become particularly dangerous.

---

## Required Condition

The account must have Kerberos preauthentication disabled.

The AD setting is commonly displayed as:

```text
Do not require Kerberos preauthentication
```

Internally, this corresponds to the:

```text
DONT_REQ_PREAUTH
```

user-account-control flag.

---

## Conceptual Attack Flow

```text
Enumerate domain users
        ↓
Identify accounts without preauthentication
        ↓
Request AS-REP
        ↓
Receive encrypted response material
        ↓
Perform offline password analysis
        ↓
Weak password may be recovered
```

---

## Important Point

AS-REP Roasting does **not** mean:

```text
Kerberos is broken
```

It generally indicates:

```text
Unsafe account configuration
        +
Potentially weak password
```

---

## Common Tools

During authorized AD labs, common tools that can identify/request AS-REP material include:

```text
Impacket
Rubeus
PowerView / AD enumeration tooling
```

A commonly associated Impacket utility is:

```text
GetNPUsers.py
```

---

## Impacket Concept

In an authorized lab, the workflow typically looks like:

```text
Known/Enumerated Users
        ↓
GetNPUsers
        ↓
KDC
        ↓
AS-REP roastable material
```

The resulting data can be analyzed offline with password-auditing tools.

---

## Rubeus

Rubeus is a Windows Kerberos interaction tool commonly encountered in AD labs.

Its AS-REP-related functionality can identify accounts where preauthentication is disabled and request the corresponding Kerberos material.

Conceptually:

```text
Rubeus
   |
   +--> Query/request
            |
            v
           KDC
            |
            v
        AS-REP data
```

---

## Detection / Enumeration

Administrators can inspect accounts for the relevant configuration.

PowerShell with the ActiveDirectory module can query user-account properties.

The important security question is:

```text
Which accounts have
Kerberos preauthentication disabled?
```

---

## Why Service Accounts Matter

A roastable account becomes especially significant when it has:

```text
Weak password
        +
Elevated permissions
```

Example:

```text
Service Account
      |
Preauth disabled
      |
Weak password
      |
Privileged group membership
```

That combination can create a serious privilege path.

---

## AS-REP Roasting vs Kerberoasting

Do not confuse them.

### AS-REP Roasting

Targets:

```text
Users without Kerberos preauthentication
```

Requires:

```text
DONT_REQ_PREAUTH
```

Conceptually attacks:

```text
AS-REP
```

### Kerberoasting

Targets:

```text
Accounts associated with SPNs
```

Conceptually attacks:

```text
Service-ticket material
```

Comparison:

```text
AS-REP Roasting
       |
       +--> Preauthentication disabled

Kerberoasting
       |
       +--> SPN/service account
```

---

## Defensive Mitigation

Primary mitigation:

```text
Require Kerberos preauthentication
```

Also:

- Use strong passwords
- Use long service-account passwords
- Audit AD userAccountControl settings
- Remove unnecessary privileged memberships
- Monitor unusual Kerberos requests
- Prefer managed service accounts where appropriate

---

## Key Takeaway

Remember:

```text
Preauthentication disabled
          ↓
AS-REQ accepted
          ↓
AS-REP returned
          ↓
Password-derived encrypted material
          ↓
Offline password guessing possible
```

AS-REP Roasting is fundamentally a combination of:

```text
Kerberos account misconfiguration
              +
Password strength
```
