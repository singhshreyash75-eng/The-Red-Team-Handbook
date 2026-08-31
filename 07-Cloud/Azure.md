# Azure

> Microsoft Azure fundamentals, identity concepts, resource hierarchy, enumeration, and cloud-security notes for authorized environments.

## What is Azure?

**Microsoft Azure** is Microsoft's cloud-computing platform.

It provides services including:

```text
Virtual Machines
Storage
Networking
Databases
Identity
Containers
Serverless Computing
Monitoring
Secrets Management
```

---

# Azure vs Microsoft Entra ID

An important distinction:

```text
Microsoft Azure
      =
Cloud infrastructure/services
```

versus:

```text
Microsoft Entra ID
(formerly Azure Active Directory)
      =
Cloud identity and access management
```

Do not automatically equate:

```text
Active Directory Domain Services
```

with:

```text
Microsoft Entra ID
```

They are related Microsoft identity technologies but have different architectures.

---

# Azure Resource Hierarchy

Conceptually:

```text
Tenant
  |
Management Groups
  |
Subscriptions
  |
Resource Groups
  |
Resources
```

Understanding this hierarchy is fundamental.

---

# Tenant

An Entra tenant represents an identity boundary.

It contains identities such as:

```text
Users
Groups
Applications
Service Principals
Managed Identities
```

Conceptually:

```text
Tenant
 |
 +-- Alice
 +-- Developers
 +-- Application
 +-- Service Principal
```

---

# Subscription

An Azure **subscription** is an important billing and resource-management boundary.

```text
Tenant
  |
  +-- Subscription A
  |
  +-- Subscription B
```

A tenant may be associated with multiple subscriptions.

---

# Resource Group

A **Resource Group** is a logical container for Azure resources.

Example:

```text
Production-RG
 |
 +-- Web VM
 +-- Database
 +-- Storage Account
 +-- Virtual Network
```

Resource Groups help with:

```text
Organization
RBAC
Deployment
Lifecycle management
```

---

# Resources

Azure resources include:

```text
Virtual Machines
Storage Accounts
Key Vaults
Databases
Virtual Networks
Web Apps
Kubernetes Clusters
Functions
```

---

# Azure CLI

Azure provides:

```text
az
```

Check:

```bash
az version
```

Authenticate interactively when appropriate:

```bash
az login
```

View current account:

```bash
az account show
```

List subscriptions:

```bash
az account list
```

Current identity/context should always be established before enumeration.

---

# Current Account Context

Useful:

```bash
az account show
```

This can identify:

```text
Subscription
Tenant
Current account context
```

Switch subscription:

```bash
az account set --subscription "<subscription>"
```

Multiple subscriptions can contain completely different resources.

---

# Microsoft Entra ID

Entra ID provides cloud identity.

Important objects:

```text
Users
Groups
Applications
Service Principals
Managed Identities
Devices
Roles
```

---

# Azure RBAC

**RBAC = Role-Based Access Control**

Azure authorization commonly follows:

```text
Security Principal
       |
Role Assignment
       |
Role Definition
       |
Scope
```

Conceptually:

```text
Alice
  |
Contributor
  |
Resource Group
```

---

# Scope

Azure RBAC permissions can apply at different levels:

```text
Management Group
      ↓
Subscription
      ↓
Resource Group
      ↓
Resource
```

Permissions assigned at a higher scope may be inherited by resources underneath it.

---

# Common Azure Roles

Examples:

```text
Owner
Contributor
Reader
User Access Administrator
```

### Reader

Generally:

```text
View resources
```

### Contributor

Generally:

```text
Manage resources
```

but does not automatically grant unrestricted ability to manage all RBAC assignments.

### Owner

Generally:

```text
Manage resources
+
Manage access
```

Highly privileged.

---

# Role Assignments

Authorized enumeration:

```bash
az role assignment list
```

Current-context information can help answer:

```text
Who has access?
Which role?
At what scope?
```

This is often more important than simply listing resources.

---

# Service Principals

Applications often need Azure identities.

A **service principal** represents an application's identity in a tenant.

Conceptually:

```text
Application
    |
Service Principal
    |
Azure Permissions
```

Service principals may authenticate using:

```text
Secrets
Certificates
Federated credentials
```

Long-lived application secrets require careful protection.

---

# Managed Identities

Azure provides:

```text
Managed Identities
```

These allow Azure resources to authenticate to Azure services without developers manually storing credentials.

Two common types:

```text
System-assigned
User-assigned
```

Conceptually:

```text
Virtual Machine
      |
Managed Identity
      |
      v
Key Vault / Storage / API
```

---

# Virtual Machines

List authorized VM resources:

```bash
az vm list
```

VMs can have:

```text
Public IPs
Private IPs
Managed identities
Disks
Network interfaces
Extensions
```

Security questions:

```text
Is it publicly reachable?
Which identity is attached?
Which network rules apply?
What secrets can it access?
```

---

# Virtual Networks

Azure networking uses:

```text
VNet = Virtual Network
```

Conceptually:

```text
VNet
 |
 +-- Subnet A
 |
 +-- Subnet B
```

Associated controls include:

```text
Network Security Groups
Route Tables
Firewalls
Private Endpoints
```

---

# NSG

**NSG = Network Security Group**

NSGs filter network traffic.

Conceptually:

```text
Source
  ↓
NSG Rule
  ↓
Allow / Deny
  ↓
Azure Resource
```

Review overly broad exposure carefully.

---

# Storage Accounts

Azure Storage supports:

```text
Blob
File
Queue
Table
```

Conceptually:

```text
Storage Account
     |
     +-- Blob Container
     +-- File Share
```

Security-sensitive areas:

```text
Public access
Access keys
SAS tokens
RBAC
Network restrictions
Encryption
```

---

# SAS

**SAS = Shared Access Signature**

A SAS grants delegated access to storage resources.

Conceptually:

```text
Storage Resource
      |
SAS Token
      |
Temporary / scoped access
```

SAS URLs/tokens should be treated as credentials.

---

# Key Vault

**Azure Key Vault** stores:

```text
Secrets
Keys
Certificates
```

Conceptually:

```text
Application
    |
Managed Identity
    |
    v
Key Vault
    |
    +-- Secret
    +-- Key
```

Security depends heavily on:

```text
RBAC/access policies
Network restrictions
Identity permissions
Secret management
```

---

# Azure Functions

Azure Functions provide serverless computing.

```text
Event
 ↓
Function
 ↓
Code
 ↓
Azure/API resources
```

Review:

```text
Managed identity
Environment/configuration
Secrets
Network exposure
Dependencies
```

---

# Azure Kubernetes Service

Azure's managed Kubernetes service is:

```text
AKS
=
Azure Kubernetes Service
```

AKS combines Azure identity/network controls with Kubernetes RBAC and workload security.

---

# Logging

Important Azure monitoring/security services include:

```text
Azure Monitor
Activity Log
Log Analytics
Microsoft Defender for Cloud
Microsoft Sentinel
```

The Azure Activity Log records subscription-level control-plane events.

---

# Common Security Risks

Examples:

```text
Overly permissive RBAC
Exposed service-principal secrets
Public storage
Overly broad NSGs
Excessive managed-identity permissions
Leaked SAS tokens
Public databases
Weak logging
Unused privileged identities
```

---

# Enumeration Methodology

When authorized access exists:

```text
Identity
   ↓
Tenant
   ↓
Subscriptions
   ↓
Role Assignments
   ↓
Resource Groups
   ↓
Resources
   ↓
Managed Identities
   ↓
Secrets / Data Access
```

Useful starting commands:

```bash
az account show
az account list
az group list
az role assignment list
```

---

# AWS vs Azure Identity

Conceptual comparison:

```text
AWS                         Azure

AWS Account                 Subscription
IAM                         Entra ID + Azure RBAC
IAM Role                    Role assignment /
                            Managed identity
Access Keys                 Service principal
                            credentials
Secrets Manager             Key Vault
CloudTrail                  Activity Log
```

These are approximate conceptual mappings, not exact equivalents.

---

# Shared Responsibility

Microsoft secures the underlying cloud platform.

Customers remain responsible for areas such as:

```text
Identity configuration
Permissions
Data
Application security
Network configuration
VM operating systems
Secrets
Access control
```

Responsibility varies by service model.

---

# Key Takeaway

Azure security is heavily centered on:

```text
IDENTITY
   +
RBAC
   +
SCOPE
   +
RESOURCES
   +
SECRETS
```

Always understand:

```text
Who is the principal?
What role does it have?
At what scope?
What resources inherit that access?
```

A small-looking role assignment at a high scope can have a very large security impact.
