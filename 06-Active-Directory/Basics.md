# Active Directory Basics

> Core concepts required before Active Directory enumeration and attack-path analysis.

## Active Directory

**Active Directory (AD)** is Microsoft's centralized directory and identity-management system.

It manages objects such as:

```text
Users
Groups
Computers
Servers
Policies
Permissions
Services
```

The most common AD component is:

```text
Active Directory Domain Services
              =
            AD DS
```

---

## Domain

A **domain** is an administrative boundary containing AD objects.

Example:

```text
corp.local
```

Possible objects:

```text
alice@corp.local
bob@corp.local

PC01.corp.local
DC01.corp.local
```

---

## Domain Controller

A **Domain Controller (DC)** provides Active Directory services.

Typical responsibilities:

```text
Authentication
Directory lookup
Kerberos
LDAP
Group Policy
Replication
DNS integration
```

Example:

```text
              corp.local
                   |
          +--------+--------+
          |                 |
        DC01              DC02
```

---

## Forest

A **forest** is the highest-level logical AD structure.

```text
               FOREST
                  |
        +---------+---------+
        |                   |
    corp.local           dev.local
```

A forest can contain multiple domains.

---

## Tree

Domains sharing a hierarchical DNS namespace can form a tree.

```text
example.com
     |
     +-- corp.example.com
     |
     +-- dev.example.com
```

---

## AD Objects

Common objects:

```text
User
Computer
Group
OU
GPO
Service account
```

Every object contains attributes.

Example:

```text
Alice
 |
 +-- username
 +-- SID
 +-- email
 +-- groups
 +-- account settings
```

---

## Organizational Units

**OU = Organizational Unit**

OUs organize objects and allow administrative delegation and Group Policy application.

```text
corp.local
 |
 +-- Employees
 |     |
 |     +-- Alice
 |     +-- Bob
 |
 +-- Servers
 |     |
 |     +-- WEB01
 |     +-- SQL01
 |
 +-- Workstations
```

---

## Groups

Groups simplify permission management.

```text
Alice ----+
Bob ------+--> IT-Users --> Resource
Charlie --+
```

Important group types/scopes include:

```text
Security
Distribution

Domain Local
Global
Universal
```

---

## Important Privileged Groups

Examples include:

```text
Domain Admins
Enterprise Admins
Administrators
Schema Admins
Account Operators
Server Operators
Backup Operators
```

Membership does not tell the entire story.

AD permissions can also come from:

```text
Nested groups
ACLs
Delegation
GPOs
Local groups
```

---

## SID

**SID = Security Identifier**

Windows identifies security principals through SIDs.

Example structure:

```text
S-1-5-21-xxxxxxxx-xxxxxxxx-xxxxxxxx-1105
```

The final value is commonly the:

```text
RID
```

Windows ACLs fundamentally reference SIDs.

---

## Distinguished Name

Example:

```text
CN=Alice,OU=Users,DC=corp,DC=local
```

Breakdown:

```text
CN = Common Name
OU = Organizational Unit
DC = Domain Component
```

---

# Important AD Protocols

## DNS

```text
Port 53
```

AD relies heavily on DNS for locating services and Domain Controllers.

---

## Kerberos

```text
Port 88
```

Primary ticket-based authentication protocol in modern AD.

Basic flow:

```text
User
 |
 v
KDC
 |
 +--> TGT
 |
 +--> Service Ticket
 |
 v
Service
```

---

## LDAP

```text
389  LDAP
636  LDAPS
```

LDAP provides access to directory information.

Conceptually:

```text
Client
  |
 LDAP query
  |
  v
Active Directory
  |
  +--> Users
  +--> Groups
  +--> Computers
```

---

## SMB

```text
Port 445
```

Used for:

```text
File sharing
Windows administration
SYSVOL
NETLOGON
IPC
```

---

## RPC

Commonly associated with:

```text
135
```

and dynamically allocated RPC ports.

Windows uses RPC extensively for administration and distributed services.

---

## Global Catalog

Common ports:

```text
3268
3269
```

The Global Catalog provides searchable information about objects across a forest.

---

# Authentication

Two major technologies:

```text
Kerberos
NTLM
```

## Kerberos

Ticket based:

```text
Credentials
    ↓
   KDC
    ↓
   TGT
    ↓
Service Ticket
    ↓
 Service
```

## NTLM

Challenge-response based:

```text
Client
   |
   | request
   v
Server
   |
   | challenge
   v
Client
   |
   | response
   v
Server
```

---

# Authentication vs Authorization

```text
Authentication
     =
Who are you?
```

versus:

```text
Authorization
     =
What can you access?
```

Successful authentication does not imply administrative access.

---

# SPN

**SPN = Service Principal Name**

SPNs associate services with accounts for Kerberos authentication.

Examples of service classes:

```text
CIFS
HTTP
LDAP
MSSQLSvc
HOST
```

SPNs become especially important when studying:

```text
Kerberoasting
```

---

# Group Policy

**GPO = Group Policy Object**

GPOs centrally configure users and computers.

Examples:

```text
Security policies
Firewall configuration
Scripts
Registry settings
Software deployment
Password/account policies
```

Conceptually:

```text
GPO
 |
 +--> Domain
 |
 +--> OU
       |
       +--> Users
       +--> Computers
```

---

# SYSVOL

Domain Controllers expose:

```text
SYSVOL
```

SYSVOL contains domain-wide files used by AD and Group Policy.

---

# NETLOGON

Another important DC share:

```text
NETLOGON
```

Often associated with domain logon-related scripts/files.

---

# Trusts

Trusts connect authentication boundaries.

```text
Domain A
   |
 TRUST
   |
Domain B
```

A trust does not automatically grant access to every resource.

---

# AD Security Mindset

Active Directory security is heavily based on relationships.

Instead of only asking:

```text
Is Alice an administrator?
```

ask:

```text
Alice
 |
MemberOf
 |
 v
Helpdesk
 |
Permission
 |
 v
SERVER01
 |
Administrative session
 |
 v
Domain Admin
```

A chain of individually valid permissions can create a powerful path.

---

# Basic Enumeration Commands

```cmd
whoami
whoami /all
whoami /groups
whoami /priv

hostname
systeminfo

echo %USERDOMAIN%
echo %LOGONSERVER%

ipconfig /all

net user /domain
net group /domain
net localgroup

klist
```

Find a DC:

```cmd
nltest /dsgetdc:<domain>
```

---

# Core AD Security Topics

After the basics, important topics include:

```text
Enumeration
BloodHound
SharpHound
Kerberoasting
AS-REP Roasting
NTLM
Pass-the-Hash
Kerberos tickets
ACL abuse
Delegation
Credential exposure
Lateral movement
```

---

## Key Takeaway

Think of Active Directory as a graph:

```text
USERS --------+
GROUPS -------+
COMPUTERS ----+
SESSIONS -----+----> RELATIONSHIPS
ACLs ---------+           |
GPOs ---------+           v
TRUSTS -------+      ACCESS PATHS
```

AD security is not just about finding privileged accounts.

It is about understanding:

```text
Objects
   +
Permissions
   +
Authentication
   +
Relationships
```
