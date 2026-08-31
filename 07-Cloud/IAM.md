# IAM

> Identity and Access Management fundamentals across cloud platforms, including users, roles, policies, permissions, and privilege relationships.

## What is IAM?

**IAM = Identity and Access Management**

IAM answers two fundamental questions:

```text
WHO are you?
```

and:

```text
WHAT are you allowed to do?
```

Cloud security is heavily dependent on IAM.

Conceptually:

```text
Identity
   |
Authentication
   |
   v
Principal
   |
Authorization
   |
   v
Resource
```

---

# Authentication vs Authorization

## Authentication

```text
Who are you?
```

Examples:

```text
Password
MFA
Access key
Certificate
Federated login
Temporary token
```

## Authorization

```text
What are you allowed to do?
```

Examples:

```text
Read bucket
Start VM
Access secret
Create user
Modify role
Delete resource
```

Remember:

```text
Authentication != Authorization
```

---

# Core IAM Model

A useful generic cloud model is:

```text
PRINCIPAL
    |
    | has
    v
ROLE / POLICY
    |
    | contains
    v
PERMISSIONS
    |
    | apply to
    v
RESOURCE
```

---

# Principal

A **principal** is an identity capable of making a request.

Examples:

```text
Human user
Group
Application
Service account
Managed identity
IAM role
Workload identity
```

---

# Human Identities

Examples:

```text
Administrator
Developer
Security Analyst
Billing User
```

Human access should preferably use:

```text
SSO
MFA
Temporary credentials
```

rather than long-lived static credentials.

---

# Workload Identities

Applications also need identities.

Examples:

```text
AWS IAM Role
Azure Managed Identity
GCP Service Account
Kubernetes Service Account
```

Conceptually:

```text
Application
     |
Workload Identity
     |
     v
Cloud API
```

This avoids embedding human credentials into applications.

---

# Users

A user generally represents a person or long-lived identity.

Depending on platform, a user may possess:

```text
Password
Access keys
Group membership
Role assignments
MFA configuration
```

---

# Groups

Groups simplify permission assignment.

Instead of:

```text
Alice -> Permission
Bob   -> Permission
John  -> Permission
```

use:

```text
Alice --+
Bob ----+--> Developers --> Permission
John ---+
```

This improves manageability.

---

# Roles

A **role** represents a set of permissions or an assumable identity, depending on the cloud platform.

Conceptually:

```text
Principal
    |
Assume / Assigned
    |
    v
Role
    |
Permissions
```

Roles are central to cloud authorization.

---

# Policies

Policies define permissions.

Generic structure:

```text
Effect
Action
Resource
Condition
```

Conceptually:

```text
ALLOW
   |
Read
   |
Bucket A
   |
Only under condition X
```

---

# Allow and Deny

IAM decisions often involve:

```text
Allow
Deny
```

A useful generic model is:

```text
No permission
     ↓
Implicit Deny
```

Explicitly permitted:

```text
Allow
```

Explicit deny:

```text
Deny
```

Platform-specific evaluation rules vary, but explicit deny mechanisms often override allows.

---

# Least Privilege

**Least Privilege** means:

```text
Give an identity
ONLY
the permissions required
to perform its job.
```

Avoid:

```text
Administrator everywhere
```

Prefer:

```text
Specific Action
      +
Specific Resource
      +
Specific Scope
      +
Specific Conditions
```

---

# Overly Broad Permissions

Dangerous examples conceptually include:

```text
Action: *
Resource: *
```

or broad administrative roles assigned unnecessarily.

These can dramatically increase the impact of credential compromise.

---

# IAM Privilege Escalation Concept

A principal may not currently be an administrator but may possess permissions allowing it to modify IAM.

Example:

```text
Developer
    |
Can modify role
    |
    v
Privileged Role
```

Therefore the real question is not only:

```text
"What resources can I access?"
```

but also:

```text
"Can I change who has access?"
```

---

# Permission Relationships

Cloud IAM is best viewed as a graph.

```text
Alice
 |
MemberOf
 |
Developers
 |
CanAssume
 |
AdminRole
 |
CanAccess
 |
Production
```

Alice may look low privileged initially.

The relationship chain reveals otherwise.

---

# Direct vs Indirect Permissions

## Direct

```text
Alice
 |
Role
 |
Resource
```

## Indirect

```text
Alice
 |
Group
 |
Role
 |
Resource
```

or:

```text
Alice
 |
Can assume Role A
 |
Role A can assume Role B
 |
Production Resource
```

Indirect relationships are extremely important during IAM review.

---

# Temporary Credentials

Modern cloud environments frequently use temporary credentials.

Advantages:

```text
Limited lifetime
Reduced static-secret exposure
Role-based access
Automatic rotation
```

Examples:

```text
AWS STS credentials
Azure access tokens
GCP short-lived credentials
```

---

# Long-Lived Credentials

Examples:

```text
AWS access keys
Service principal secrets
GCP service-account JSON keys
```

Risks:

```text
Git leaks
CI/CD leaks
Developer laptop compromise
Old unused credentials
No automatic rotation
```

Prefer short-lived credentials where practical.

---

# MFA

**MFA = Multi-Factor Authentication**

MFA provides additional protection for human authentication.

Conceptually:

```text
Password
   +
Second Factor
   ↓
Authentication
```

Particularly important for:

```text
Administrators
Root/owner accounts
Privileged users
Cloud consoles
```

---

# Federation

Organizations often use a central identity provider.

Conceptually:

```text
Employee
   |
Identity Provider
   |
Federation
   |
   v
Cloud Provider
   |
Temporary Role
```

Benefits include:

```text
Centralized identity lifecycle
SSO
MFA
Reduced static cloud accounts
```

---

# AWS IAM

Important AWS concepts:

```text
IAM Users
Groups
Roles
Policies
STS
Trust Policies
Resource Policies
```

Current principal:

```bash
aws sts get-caller-identity
```

The critical role concept:

```text
Principal
   |
AssumeRole
   |
   v
IAM Role
```

---

# Azure IAM

Azure commonly combines:

```text
Microsoft Entra ID
        +
Azure RBAC
```

Model:

```text
Principal
   |
Role Assignment
   |
Role Definition
   |
Scope
```

Scopes:

```text
Management Group
Subscription
Resource Group
Resource
```

---

# GCP IAM

GCP model:

```text
Principal
   |
Role
   |
Resource
```

Hierarchy:

```text
Organization
    ↓
Folder
    ↓
Project
    ↓
Resource
```

Permissions can be inherited from higher levels.

---

# IAM Comparison

```text
AWS                 Azure                 GCP

IAM User            Entra User            Google Identity
IAM Group           Entra Group           Google Group
IAM Role            Azure Role /          Service Account /
                    Managed Identity       IAM Role
Policy              Role Definition       IAM Policy
STS                 Access Token          Short-lived Token
```

These are conceptual comparisons rather than exact equivalents.

---

# Resource-Based Permissions

Some cloud resources can contain their own access policies.

Examples include:

```text
Storage buckets
Secrets
Encryption keys
Queues
Functions
```

Therefore access can come from:

```text
Identity policy
       +
Resource policy
```

depending on the platform.

---

# IAM and Secrets

An identity may not have administrator privileges but could still have access to:

```text
Secrets
API keys
Database passwords
Signing keys
Service credentials
```

Those credentials may lead to additional access.

Conceptually:

```text
Low-Privilege Identity
        |
CanReadSecret
        |
        v
Privileged Credential
        |
        v
Higher Privilege
```

---

# IAM and Compute

Workloads commonly have attached identities.

Examples:

```text
EC2 -> IAM Role

Azure VM -> Managed Identity

GCE VM -> Service Account
```

Therefore:

```text
Compromise VM
      ↓
Access workload identity
      ↓
Cloud API permissions
```

The workload identity should always follow least privilege.

---

# IAM and Serverless

Functions also have identities.

```text
Lambda
Azure Function
Cloud Function
```

Conceptually:

```text
Function
   |
Execution Identity
   |
   v
Cloud Resources
```

A vulnerability in application code can become a cloud-security issue if the execution identity is overprivileged.

---

# IAM and Containers

Containers may receive cloud identity through:

```text
Instance role
Managed identity
Service account
Workload identity
Mounted credentials
Environment variables
```

Therefore:

```text
Container compromise
        ↓
Identity compromise
        ↓
Cloud-resource access
```

---

# Common IAM Misconfigurations

Look for:

```text
Administrator roles assigned broadly
Wildcard permissions
Unused privileged accounts
Long-lived credentials
No MFA
Overprivileged service accounts
Unsafe role trust
Public resource policies
Excessive secret access
Cross-account misconfiguration
Poor permission inheritance
```

---

# Enumeration Methodology

Always start with:

```text
WHO AM I?
```

Then:

```text
Current Principal
       ↓
Direct Roles
       ↓
Group Memberships
       ↓
Inherited Permissions
       ↓
Assumable Roles
       ↓
Resource Policies
       ↓
Secrets
       ↓
Privilege Relationships
```

---

# IAM Security Review Questions

Ask:

```text
Who has administrative access?

Who can modify IAM?

Who can create credentials?

Who can assume privileged roles?

Who can read secrets?

Who controls compute identities?

Who can modify serverless code?

Which permissions are inherited?

Which credentials are long-lived?

Which accounts lack MFA?
```

---

# Privilege vs Permission

Do not focus only on obvious administrator roles.

Example:

```text
User cannot administer production
```

but:

```text
User can modify function code
        ↓
Function uses privileged identity
```

This relationship may be security-sensitive.

Cloud privilege is often **indirect**.

---

# IAM Attack-Path Mindset

Think:

```text
Principal
   |
Permission
   |
   v
Resource / Identity
   |
Permission
   |
   v
Another Resource
```

This is similar to Active Directory BloodHound-style thinking:

```text
Objects
   +
Relationships
   =
Privilege Paths
```

---

# Defensive Best Practices

Use:

```text
Least privilege
MFA
SSO/federation
Temporary credentials
Regular access reviews
Credential rotation
Permission boundaries
Centralized logging
Separation of duties
Workload identities
Secret managers
```

Remove:

```text
Unused users
Unused roles
Old access keys
Unnecessary administrator rights
Overly broad policies
```

---

# Key Takeaway

Cloud IAM is fundamentally:

```text
WHO
 |
CAN DO
 |
WHAT
 |
TO WHICH RESOURCE
 |
UNDER WHAT CONDITIONS
```

When reviewing cloud security, do not stop at:

```text
"What permissions does this user have?"
```

Also determine:

```text
What identities can they influence?
What roles can they assume?
What secrets can they access?
What workloads can they modify?
What permissions can they indirectly obtain?
```

In cloud environments:

```text
IAM IS THE SECURITY PERIMETER.
```
