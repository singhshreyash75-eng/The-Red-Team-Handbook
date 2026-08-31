# BloodHound

> Active Directory relationship and attack-path analysis for authorized security assessments and labs.

## What is BloodHound?

**BloodHound** analyzes relationships inside Active Directory.

Instead of viewing AD as simple lists of:

```text
Users
Groups
Computers
```

BloodHound represents AD as a **graph**.

```text
Nodes
  +
Edges
  =
Graph
```

This makes complex privilege relationships easier to identify.

---

# Graph Concept

Example:

```text
Alice
  |
MemberOf
  v
Helpdesk
  |
AdminTo
  v
SERVER01
  |
Session
  v
Administrator
```

Individually, Alice may appear unprivileged.

But the relationship chain can reveal a path toward privileged access.

---

# Nodes

A **node** represents an AD object.

Examples:

```text
User
Group
Computer
Domain
GPO
OU
Container
```

Conceptually:

```text
(Alice)
   |
   |
(Helpdesk)
   |
   |
(SERVER01)
```

---

# Edges

An **edge** represents a relationship between nodes.

Examples include:

```text
MemberOf
AdminTo
HasSession
GenericAll
GenericWrite
WriteDacl
WriteOwner
Owns
CanRDP
CanPSRemote
```

Exact available edge types depend on BloodHound version and collected data.

---

# Example Graph

```text
[alice]
   |
 MemberOf
   |
   v
[IT-Support]
   |
 AdminTo
   |
   v
[WS01]
```

This means:

```text
alice
   ↓
belongs to IT-Support
   ↓
IT-Support has administrative rights on WS01
```

---

# Why BloodHound is Powerful

Traditional enumeration might produce:

```text
5000 users
1000 groups
3000 computers
Thousands of permissions
```

Manually correlating everything becomes difficult.

BloodHound asks:

```text
How are these objects connected?
```

Graph algorithms can then identify paths between objects.

---

# BloodHound Architecture

Conceptually:

```text
Active Directory
       |
       |
Data Collector
       |
       v
Collected AD Data
       |
       v
BloodHound
       |
       v
Graph Analysis
```

In traditional BloodHound workflows, a common collector is:

```text
SharpHound
```

---

# SharpHound

**SharpHound** collects AD relationship information for BloodHound analysis.

Conceptually:

```text
Domain
  |
  | enumeration
  v
SharpHound
  |
  v
Collected data
  |
  v
BloodHound
```

Depending on configuration/version, collection can include information about:

```text
Users
Groups
Computers
Sessions
Local administrators
ACLs
Trusts
OUs
GPO relationships
```

---

# Collection vs Analysis

Keep these separate:

```text
SharpHound
    =
Data Collection
```

```text
BloodHound
    =
Graph Analysis
```

This distinction is important.

---

# Common Relationship: MemberOf

```text
Alice
  |
MemberOf
  v
IT
```

Meaning:

```text
Alice is a member of IT
```

Nested memberships can create longer chains:

```text
Alice
  |
MemberOf
  v
Helpdesk
  |
MemberOf
  v
IT-Admins
```

---

# AdminTo

Conceptually:

```text
User / Group
     |
   AdminTo
     |
     v
 Computer
```

It indicates administrative control of a machine in the modeled data.

Example:

```text
IT-Admins
    |
 AdminTo
    |
    v
 SERVER01
```

---

# HasSession

Conceptually:

```text
Computer
    |
 HasSession
    |
    v
   User
```

Sessions matter because privileged users logging into lower-trust systems can create credential-exposure risk.

Example:

```text
SERVER01
    |
 HasSession
    |
    v
DomainAdmin
```

The precise interpretation and collection reliability should always be checked against the BloodHound version/data source.

---

# ACL Relationships

BloodHound is especially useful for identifying dangerous AD permissions.

Important examples include:

```text
GenericAll
GenericWrite
WriteDacl
WriteOwner
Owns
```

---

## GenericAll

Conceptually represents broad control over an object.

```text
User A
   |
GenericAll
   |
   v
Object B
```

Security impact depends on what Object B is.

---

## GenericWrite

Provides significant ability to modify attributes of an object.

```text
User
  |
GenericWrite
  |
  v
Object
```

The exact security consequence depends on the target object's type and writable attributes.

---

## WriteDacl

Allows modification of an object's discretionary access-control list.

Conceptually:

```text
User
 |
WriteDacl
 |
 v
Object
```

Since ACLs determine permissions, this relationship can be highly security-sensitive.

---

## WriteOwner

Allows changing an object's ownership.

```text
User
 |
WriteOwner
 |
 v
Object
```

Ownership can influence subsequent permission management.

---

# Paths

The central BloodHound concept is a **path**.

Example:

```text
Alice
 |
MemberOf
 |
 v
Helpdesk
 |
GenericAll
 |
 v
Server-Admins
 |
AdminTo
 |
 v
DC-Management-System
```

BloodHound helps reveal chains that would be difficult to recognize manually.

---

# Shortest Paths

One common analytical question is:

```text
What is the shortest path
from this principal
to a privileged target?
```

Conceptually:

```text
LOW PRIVILEGE
     |
     | relationship
     v
   GROUP
     |
     | permission
     v
 COMPUTER
     |
     | relationship
     v
HIGH PRIVILEGE
```

A short path does not automatically mean exploitation is trivial.

Every edge must be validated.

---

# High-Value Targets

Examples of high-value objects may include:

```text
Domain Admins
Enterprise Admins
Domain Controllers
Tier-0 systems
Highly privileged service accounts
Critical administrative groups
```

What counts as high value depends on the environment.

---

# BloodHound Questions

Useful analysis questions include:

```text
Who has control over this group?

Who can administer this computer?

Which users have paths to privileged groups?

Where do privileged users have sessions?

Which objects have dangerous ACL relationships?

Which groups are nested into privileged groups?

Which systems create administrative bridges?
```

---

# BloodHound Is Not an Exploit

Important:

```text
BloodHound != Exploitation tool
```

Its core purpose is:

```text
Collect/ingest relationships
        ↓
Represent as graph
        ↓
Analyze privilege paths
```

It helps identify where security boundaries may be weak.

---

# Validate Findings

Do not blindly trust every graph edge.

Always verify important findings.

```text
BloodHound finding
       ↓
Understand relationship
       ↓
Verify manually
       ↓
Determine actual permissions
       ↓
Assess security impact
```

Reasons include:

```text
Stale data
Changed permissions
Old sessions
Collection limitations
Environmental changes
Misinterpreted relationships
```

---

# Defensive Use

BloodHound is extremely useful for defenders.

Defenders can use graph analysis to identify:

```text
Excessive privileges
Dangerous ACLs
Nested privileged groups
Administrative overlap
Credential exposure paths
Unnecessary local admin rights
Weak tiering boundaries
```

The goal is to reduce:

```text
Number of paths
        +
Length/reachability of dangerous paths
```

---

# Example Defensive Finding

Suppose:

```text
Helpdesk
   |
AdminTo
   v
SERVER01
   |
HasSession
   v
Domain Admin
```

Even if Helpdesk is intentionally allowed to administer SERVER01, having highly privileged users use that same machine can create an undesirable administrative-tier relationship.

BloodHound makes this type of structural problem visible.

---

# BloodHound Mindset

Do not think:

```text
"Find Domain Admin password"
```

Think:

```text
What relationships allow
one security principal
to influence another?
```

---

# AD as a Graph

Traditional view:

```text
Users
Groups
Computers
```

BloodHound view:

```text
USER
 |
MemberOf
 |
GROUP
 |
AdminTo
 |
COMPUTER
 |
HasSession
 |
USER
 |
Control
 |
GROUP
```

That is the key conceptual shift.

---

# BloodHound vs SharpHound

```text
SharpHound
    |
    | collects
    v
AD relationship data
    |
    | imported/ingested
    v
BloodHound
    |
    | analyzes
    v
Graph / Paths
```

Remember:

```text
SharpHound = Collector

BloodHound = Analyzer
```

---

## Key Takeaway

BloodHound converts Active Directory from:

```text
Huge lists of objects
```

into:

```text
Objects
   +
Relationships
   ↓
Graph
   ↓
Privilege Paths
```

The fundamental security idea is:

```text
A low-privileged account
does not need to be directly privileged
if a chain of permissions and relationships
connects it to a high-value target.
```

That relationship-based mindset is one of the most important concepts in Active Directory security.
