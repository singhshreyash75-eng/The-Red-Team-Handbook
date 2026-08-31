# SharpHound

> Active Directory data collection for BloodHound analysis in authorized environments.

## What is SharpHound?

**SharpHound** is a data collector associated with **BloodHound**.

Its job is to collect Active Directory relationship information.

The basic workflow is:

```text
Active Directory
       ↓
   SharpHound
       ↓
Collected relationship data
       ↓
   BloodHound
       ↓
Graph analysis
```

Remember:

```text
SharpHound = Collector

BloodHound = Analyzer
```

---

# Why SharpHound Exists

AD can contain:

```text
Thousands of users
Thousands of computers
Hundreds of groups
Millions of permission relationships
```

Manual correlation becomes difficult.

SharpHound automates collection of relationship data that BloodHound can model as a graph.

---

# What Does SharpHound Collect?

Depending on version, permissions, and collection configuration, data can include:

```text
Users
Groups
Computers
Group memberships
Sessions
Local administrators
ACLs
Trusts
OUs
GPO relationships
Remote-management rights
```

Not every collection method obtains every data type.

---

# Graph Data

Suppose AD contains:

```text
Alice
   |
MemberOf
   |
Helpdesk
   |
AdminTo
   |
WS01
```

SharpHound gathers relevant relationship information.

BloodHound then represents it as:

```text
(Alice)
   |
MemberOf
   |
   v
(Helpdesk)
   |
 AdminTo
   |
   v
(WS01)
```

---

# Nodes and Edges

## Nodes

Objects such as:

```text
Users
Groups
Computers
Domains
OUs
GPOs
```

## Edges

Relationships such as:

```text
MemberOf
AdminTo
HasSession
CanRDP
CanPSRemote
GenericAll
GenericWrite
WriteDacl
WriteOwner
```

Available relationship types depend on BloodHound/SharpHound versions and collected information.

---

# Collection Methods

SharpHound supports different collection approaches.

Conceptually:

```text
Collection Method
       ↓
Select AD information
       ↓
Enumerate
       ↓
Output dataset
```

Collection scope should be limited to what is required and authorized.

---

# Group Collection

Group membership is fundamental.

Example:

```text
Alice
 |
MemberOf
 |
Helpdesk
 |
MemberOf
 |
IT
```

Nested memberships matter because privileges can propagate through groups.

---

# Computer Information

Computer objects help map:

```text
Workstations
Servers
Domain Controllers
Administrative systems
```

Relationships between users/groups and computers can reveal administrative paths.

---

# Local Administrator Relationships

Suppose:

```text
IT-Support
     |
   AdminTo
     |
     v
   WS01
```

This relationship means the group has administrative control over that system according to the collected/modelled data.

This can become significant when combined with other relationships.

---

# Session Information

Session information can help identify where identities are logged on.

Example:

```text
WS01
 |
HasSession
 |
 v
Alice
```

A particularly important scenario is:

```text
Low-trust machine
      +
Privileged user session
```

because privileged credentials or authentication artifacts may become exposed if the machine is compromised.

Session information can be incomplete or transient and must be validated.

---

# ACL Collection

AD objects contain Access Control Lists.

SharpHound/BloodHound can model relationships such as:

```text
GenericAll
GenericWrite
WriteDacl
WriteOwner
Owns
```

Example:

```text
Alice
 |
GenericWrite
 |
 v
Bob
```

The security impact depends on the object type and exact permissions.

---

# Trust Collection

SharpHound can help map domain/forest relationships.

Conceptually:

```text
Domain A
    |
  Trust
    |
    v
Domain B
```

Trusts matter because they can extend authentication relationships beyond a single domain.

---

# GPO Relationships

Group Policy can affect:

```text
Users
Computers
OUs
```

Conceptually:

```text
GPO
 |
LinkedTo
 |
 v
OU
 |
Contains
 |
 v
Computers
```

Understanding these relationships can reveal where administrative policy influence exists.

---

# SharpHound Output

Collected information is prepared for ingestion into BloodHound.

Conceptually:

```text
SharpHound
     ↓
Collection output
     ↓
BloodHound ingestion
     ↓
Graph database / analysis
```

The exact output format depends on the BloodHound generation/version.

---

# Operational Considerations

Collection can generate:

```text
LDAP queries
SMB connections
RPC activity
Host enumeration
Directory queries
```

Some collection methods are more network-intensive than others.

Therefore, during authorized assessments:

```text
Define scope
   ↓
Choose required collection
   ↓
Minimize unnecessary traffic
   ↓
Collect
   ↓
Analyze
```

---

# SharpHound Is Not BloodHound

A frequent beginner mistake:

```text
SharpHound == BloodHound
```

Incorrect.

Use:

```text
SharpHound
     |
Collects AD data
     |
     v
BloodHound
     |
Analyzes relationships
```

---

# Validation

BloodHound data represents the state observed during collection.

Environments change.

Therefore:

```text
Collected Edge
      ↓
Potential relationship
      ↓
Manual verification
      ↓
Confirmed current relationship
```

Possible reasons for stale/inaccurate data:

```text
Permission changes
Group membership changes
Session changes
Computer offline
Collection limitations
Old datasets
```

---

# Defensive Use

SharpHound/BloodHound are valuable defensive tools.

Blue teams can identify:

```text
Excessive local administrators
Privileged sessions on unsafe systems
Dangerous ACLs
Nested privileged groups
Weak administrative tiering
Unexpected trust paths
Excessive remote-management rights
```

---

# Example Defensive Analysis

Suppose graph data shows:

```text
Helpdesk
   |
AdminTo
   |
WS01
   |
HasSession
   |
Domain Admin
```

Potential concern:

```text
Helpdesk controls WS01
         +
Domain Admin uses WS01
         ↓
Administrative-tier exposure
```

The remediation may involve changing where privileged users log in or reducing unnecessary administrative rights.

---

# Collection Mindset

Do not collect everything simply because it is possible.

Ask:

```text
What question am I trying to answer?
```

Examples:

```text
Who controls privileged groups?

Where do privileged users have sessions?

Which users administer servers?

Which ACLs create privilege paths?

Which systems bridge security tiers?
```

Then collect the data necessary to answer those questions.

---

# Tool Chain

```text
          ACTIVE DIRECTORY
                 |
                 v
            SharpHound
                 |
          Relationship Data
                 |
                 v
            BloodHound
                 |
        Graph / Path Analysis
                 |
                 v
       Manual Verification
```

---

# Key Takeaway

Remember:

```text
SharpHound
    =
Collection
```

```text
BloodHound
    =
Analysis
```

Together they transform:

```text
AD Objects
    +
AD Relationships
       ↓
Graph
       ↓
Privilege-path visibility
```
