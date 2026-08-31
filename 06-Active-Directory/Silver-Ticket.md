# Silver Ticket

> Kerberos Silver Ticket concepts, prerequisites, differences from Golden Tickets, and defensive understanding.

## What is a Silver Ticket?

A **Silver Ticket** is a forged Kerberos **service ticket**.

Instead of compromising the domain-wide KRBTGT secret, a Silver Ticket relies on key material belonging to the account associated with a particular Kerberos service.

Conceptually:

```text
Service-account key compromised
           ↓
Forge service ticket
           ↓
Impersonate identity to that service
```

---

# Kerberos Refresher

Normal Kerberos:

```text
User
 |
 | AS-REQ
 v
KDC
 |
 | TGT
 v
User
 |
 | TGS-REQ
 v
KDC
 |
 | Service Ticket
 v
User
 |
 | Present ticket
 v
Service
```

Normally the KDC issues the service ticket.

---

# Silver Ticket Concept

If the key associated with the target service is compromised:

```text
Service Account Key
        ↓
Forge Service Ticket
        ↓
Present directly to Service
```

This differs fundamentally from forging a TGT.

---

# Service Account

Kerberos services are represented through SPNs.

Example:

```text
MSSQLSvc/sql01.corp.local:1433
```

An account is associated with this service.

Conceptually:

```text
Service Account
      |
      +--> SPN
             |
             v
          Service
```

The service account's key participates in protecting Kerberos service tickets intended for that service.

---

# Why the Name "Silver Ticket"?

The terminology distinguishes it from:

```text
Golden Ticket
```

Golden:

```text
KRBTGT
   ↓
TGT
   ↓
Broad domain Kerberos impact
```

Silver:

```text
Service account
      ↓
Service Ticket
      ↓
Specific service scope
```

---

# Golden vs Silver Ticket

| Golden Ticket | Silver Ticket |
|---|---|
| Forged TGT | Forged service ticket |
| Uses KRBTGT key | Uses service-account/computer key |
| Domain-level Kerberos significance | Service-specific significance |
| TGT stage | Service-ticket stage |
| Severe domain compromise prerequisite | Service credential compromise prerequisite |

Mental model:

```text
          KERBEROS
             |
      +------+------+
      |             |
     TGT        Service Ticket
      |             |
   Golden         Silver
   Ticket         Ticket
```

---

# Required Prerequisite

The important prerequisite is:

```text
Service account/computer
Kerberos key material
```

Additional contextual information is normally required to construct a valid ticket for the intended environment.

The exact requirements depend on:

```text
Domain
Service
Account
Encryption type
Target
```

---

# Computer Accounts

Not every Kerberos service runs under a human-created service account.

Many Windows services use:

```text
Computer account
```

Example:

```text
SERVER01$
```

Computer accounts also possess credentials/keys.

Therefore service-ticket security can involve either:

```text
User service account

or

Computer account
```

depending on the SPN/service configuration.

---

# SPN

**SPN = Service Principal Name**

It identifies a Kerberos service.

Common service classes include:

```text
CIFS
HTTP
HOST
LDAP
MSSQLSvc
```

Conceptually:

```text
CIFS/fileserver.corp.local
```

The service represented by the ticket determines what the ticket can be used for.

---

# Scope

A Silver Ticket is generally narrower than a Golden Ticket.

Example:

```text
Compromised key for Service A
             ↓
Forged ticket for Service A
```

does not automatically mean:

```text
Valid ticket for every service
```

This service-specific nature is a central Silver Ticket concept.

---

# KDC Interaction

Normal service-ticket acquisition:

```text
Client
   ↓
KDC
   ↓
Service Ticket
   ↓
Service
```

Silver Ticket concept:

```text
Service key already compromised
          ↓
Service ticket forged
          ↓
Target service
```

This distinction has important detection implications because the normal ticket-granting workflow may not occur in the expected way.

---

# Silver Ticket vs Kerberoasting

These concepts involve service accounts but are very different.

## Kerberoasting

```text
SPN
 ↓
Request legitimate service ticket
 ↓
Offline password guessing
 ↓
Potentially recover service credential
```

## Silver Ticket

```text
Service credential/key
already compromised
 ↓
Forge service ticket
```

Therefore Kerberoasting could theoretically contribute to obtaining service-account credentials, while Silver Ticket abuse occurs **after relevant key material is already compromised**.

---

# Silver Ticket vs Pass-the-Ticket

Also different.

## Pass-the-Ticket

```text
Existing valid Kerberos ticket
          ↓
Reuse it
```

## Silver Ticket

```text
Service key
     ↓
Forge new service ticket
```

Comparison:

```text
Pass-the-Ticket
      =
Reuse ticket

Silver Ticket
      =
Forge service ticket
```

---

# Tools

Security research tools commonly associated with Kerberos ticket analysis include:

```text
Mimikatz
Rubeus
Impacket
```

In an authorized lab, these may be used to demonstrate ticket construction and authentication behavior.

The critical concept remains:

```text
Which key protects which Kerberos ticket?
```

---

# Detection Challenges

Silver Tickets can be challenging to detect because authentication may be presented directly to a service.

Defenders should correlate:

```text
Service access
Kerberos activity
Account activity
Endpoint logs
Domain Controller logs
Ticket properties
```

Look for inconsistencies rather than relying on one event.

---

# Potential Indicators

Depending on the environment and implementation, suspicious patterns may include:

```text
Service access without expected preceding activity
Unexpected identity/service combinations
Abnormal ticket properties
Unusual privilege claims
Compromised service-account activity
Unexpected Kerberos encryption behavior
```

These are investigation leads, not automatic proof.

---

# Service Account Protection

Because service-account key compromise is central to Silver Ticket risk:

```text
Protect service credentials
```

Use:

- Strong random passwords
- gMSAs where appropriate
- Least privilege
- Credential rotation
- Restricted administrative access
- Secure service configuration
- Monitoring for credential theft

---

# gMSA

**gMSA = Group Managed Service Account**

A gMSA provides automatically managed service credentials.

Conceptually:

```text
Traditional Service Account
           |
    Human-managed password
           |
    May become old/weak
```

versus:

```text
gMSA
 |
Automatically managed
high-entropy credential
```

This can substantially improve service-account credential hygiene.

---

# Credential Rotation

If service-account key material is confirmed compromised:

```text
Compromised service credential
          ↓
Rotate credential
          ↓
Old key invalidated
          ↓
Previously forged material loses usefulness
```

Exact remediation must account for service dependencies and Kerberos behavior.

---

# Detection + Response

If Silver Ticket activity is suspected:

```text
Identify target service
        ↓
Identify associated account
        ↓
Investigate credential exposure
        ↓
Rotate affected credentials
        ↓
Review privileged access
        ↓
Investigate lateral movement
```

Do not treat the forged ticket as the only problem.

The more fundamental issue is:

```text
How was the service key compromised?
```

---

# Ticket Attack Comparison

```text
                 Kerberos Ticket Abuse
                         |
        +----------------+----------------+
        |                |                |
   Pass-the-Ticket    Golden           Silver
        |             Ticket           Ticket
        |                |                |
Existing Ticket      KRBTGT Key       Service Key
        |                |                |
Reuse Ticket         Forge TGT      Forge Service
                                       Ticket
```

---

# Kerberos Attack Sequence

The concepts from this module now fit together:

```text
AS-REP Roasting
      |
Potential user credential
      v

Kerberoasting
      |
Potential service credential
      v

Credential Exposure
      |
      +--> Pass-the-Hash
      |
      +--> Pass-the-Ticket
      |
      +--> Service key
      |       ↓
      |   Silver Ticket
      |
      +--> KRBTGT key
              ↓
          Golden Ticket
```

These are not necessarily sequential attack steps; the diagram shows how the credential/ticket concepts relate.

---

# Defensive Priorities

Protect:

```text
Domain Controllers
KRBTGT secrets
Service accounts
Computer-account credentials
Privileged identities
Kerberos tickets
Administrative workstations
```

Combine:

```text
Strong credentials
       +
Least privilege
       +
Administrative tiering
       +
Credential protection
       +
Kerberos monitoring
```

---

# Key Takeaway

Memorize this distinction:

```text
Golden Ticket
      =
KRBTGT key
      ↓
Forged TGT
      ↓
Broad domain significance
```

```text
Silver Ticket
      =
Service key
      ↓
Forged Service Ticket
      ↓
Specific service significance
```

Both indicate serious credential/key compromise, but they operate at **different layers of the Kerberos ticket hierarchy**.
