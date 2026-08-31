# Golden Ticket

> Kerberos Golden Ticket concepts, prerequisites, detection, and defensive understanding for authorized Active Directory labs.

## What is a Golden Ticket?

A **Golden Ticket** is a forged Kerberos **Ticket Granting Ticket (TGT)**.

It becomes possible when an attacker has obtained the secret associated with the domain's:

```text
KRBTGT account
```

Conceptually:

```text
KRBTGT secret compromised
          ↓
Ability to forge TGTs
          ↓
KDC can accept forged ticket data
          ↓
Impersonation / domain-level impact
```

This is a **post-compromise technique**.

It is not an initial-access vulnerability.

---

# KRBTGT Account

Every AD domain has a special account:

```text
krbtgt
```

It is used by the Kerberos Key Distribution Center when protecting TGTs.

Simplified:

```text
User
 |
 | authentication
 v
KDC
 |
 | TGT protected using
 v
KRBTGT-derived key
```

Therefore compromise of KRBTGT key material is extremely serious.

---

# Normal Kerberos Flow

Normally:

```text
User credentials
      ↓
     KDC
      ↓
     TGT
      ↓
Service-ticket request
      ↓
Service Ticket
      ↓
Target Service
```

The KDC issues the TGT.

---

# Golden Ticket Concept

With compromised KRBTGT key material:

```text
KRBTGT key
     ↓
Forge TGT
     ↓
Specify identity/authorization information
     ↓
Use forged TGT in Kerberos workflow
```

The crucial difference is:

```text
Normal TGT
   =
Issued through normal KDC authentication

Golden Ticket
   =
Forged using compromised domain Kerberos secret
```

---

# Why is it Critical?

The KRBTGT account exists at the **domain level**.

Compromise can therefore have domain-wide implications.

Potential security consequences include:

```text
Identity impersonation
Persistent Kerberos access
Privilege impersonation
Access to domain resources
Long-term persistence
```

Exact access still depends on environment and ticket construction.

---

# Required Prerequisites

A Golden Ticket is not created merely from a normal user's password.

The critical prerequisite is access to:

```text
KRBTGT key material
```

Additional domain information is normally relevant, such as:

```text
Domain name
Domain SID
Kerberos encryption/key information
Identity/group information
```

Obtaining KRBTGT secrets generally implies that the environment has already suffered a major compromise.

---

# Domain SID

A domain SID looks approximately like:

```text
S-1-5-21-XXXXXXXXXX-XXXXXXXXXX-XXXXXXXXXX
```

Users/groups then have RIDs appended.

Conceptually:

```text
Domain SID
   +
RID
   ↓
Principal SID
```

Example structure:

```text
S-1-5-21-AAA-BBB-CCC-1105
```

---

# Golden Ticket vs Silver Ticket

Do not confuse these.

## Golden Ticket

Based on:

```text
KRBTGT secret
```

Creates:

```text
Forged TGT
```

Scope:

```text
Domain Kerberos authentication
```

---

## Silver Ticket

Based on:

```text
Specific service-account/computer key
```

Creates:

```text
Forged service ticket
```

Scope is generally tied to:

```text
Specific service(s)
```

Comparison:

```text
Golden Ticket
    |
KRBTGT key
    |
Forged TGT
    |
Broad domain impact


Silver Ticket
    |
Service key
    |
Forged service ticket
    |
Service-specific impact
```

---

# Tools Associated With Golden Tickets

In authorized AD labs you may encounter:

```text
Mimikatz
Rubeus
Impacket
```

These tools provide Kerberos functionality, including ticket inspection/manipulation in appropriate testing environments.

Understanding the underlying Kerberos model is more important than memorizing tool syntax.

---

# Ticket Inspection

Windows cached Kerberos tickets:

```cmd
klist
```

This helps inspect:

```text
Client principal
Service principal
Ticket lifetime
Encryption type
Cached tickets
```

---

# Detection Perspective

Golden Ticket detection can be difficult because a forged ticket may appear as Kerberos activity.

Defenders should correlate:

```text
Authentication events
Ticket properties
Account activity
Privilege usage
Endpoint telemetry
Domain Controller logs
```

Potential anomalies include:

```text
Unusual ticket lifetime
Unexpected account behavior
Impossible/inconsistent identity data
Unexpected privileged access
Kerberos activity inconsistent with normal authentication
```

No single indicator should automatically be treated as proof.

---

# Incident Response

If KRBTGT key material is confirmed compromised, simply changing the affected user's password is insufficient.

The KRBTGT account has current and previous password/key history used during ticket transitions.

Standard AD recovery guidance commonly involves **two controlled KRBTGT password resets**, allowing replication and operational considerations between them.

This should be performed through a carefully planned domain recovery procedure.

---

# Defensive Mitigation

Protect the path to KRBTGT compromise:

```text
Protect Domain Controllers
        ↓
Protect Tier-0 accounts
        ↓
Restrict administrative logons
        ↓
Use credential protections
        ↓
Monitor privileged activity
```

Also:

- Minimize Domain Admin usage
- Protect DC backups
- Apply administrative tiering
- Monitor credential-dumping activity
- Patch Domain Controllers
- Restrict interactive logons to DCs
- Protect highly privileged credentials

---

# Key Takeaway

Remember:

```text
KRBTGT secret compromised
          ↓
Kerberos trust anchor compromised
          ↓
Forged TGT possible
          ↓
Golden Ticket
```

A Golden Ticket is primarily a **post-domain-compromise persistence and impersonation technique**.

If KRBTGT material has been exposed, treat it as a severe domain-level incident.
