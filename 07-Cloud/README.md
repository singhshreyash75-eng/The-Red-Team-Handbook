# Cloud Services

> Cloud security fundamentals covering AWS, Azure, GCP, IAM, Docker, Kubernetes, and cloud storage.

## Module Overview

Modern cloud security is primarily built around:

```text
IDENTITY
    +
PERMISSIONS
    +
RESOURCES
    +
NETWORKING
    +
DATA
    +
WORKLOADS
```

This module covers three major cloud providers:

```text
AWS
Azure
GCP
```

along with:

```text
IAM
Docker
Kubernetes
Cloud Storage
```

---

# Files

```text
AWS.md
Azure.md
Docker.md
GCP.md
IAM.md
Kubernetes.md
README.md
S3.md
```

---

# Cloud Service Models

Three important service models:

```text
IaaS
PaaS
SaaS
```

---

## IaaS

**Infrastructure as a Service**

Provider supplies infrastructure while the customer manages much of the operating environment.

Examples:

```text
AWS EC2
Azure Virtual Machines
GCP Compute Engine
```

Conceptually:

```text
Cloud Provider
     |
Hardware
Networking
Virtualization
     |
Customer
     |
Operating System
Applications
Data
Configuration
```

---

## PaaS

**Platform as a Service**

The provider manages more of the underlying platform.

Examples include managed:

```text
Databases
Application platforms
Serverless platforms
```

The customer focuses more heavily on:

```text
Application
Data
Identity
Configuration
```

---

## SaaS

**Software as a Service**

The provider operates the application.

Examples conceptually include:

```text
Email platforms
CRM systems
Collaboration platforms
```

Customer security responsibilities still include areas such as:

```text
Accounts
Permissions
Data
MFA
Configuration
```

---

# Shared Responsibility Model

One of the most important cloud concepts:

```text
Cloud Provider
      |
Security OF the Cloud
```

versus:

```text
Customer
      |
Security IN the Cloud
```

The exact division depends on the service.

---

# Cloud Identity

Cloud environments rely heavily on identity.

Common identity concepts:

```text
AWS
 |
IAM Users
IAM Roles
STS


Azure
 |
Entra ID
Service Principals
Managed Identities


GCP
 |
Google Identities
Service Accounts
Workload Identities
```

---

# IAM

**IAM = Identity and Access Management**

Generic model:

```text
Principal
    |
    v
Role / Policy
    |
    v
Permissions
    |
    v
Resource
```

The key question is:

```text
WHO
can perform
WHAT
on
WHICH RESOURCE?
```

---

# Least Privilege

Cloud identities should receive only the permissions they require.

Avoid:

```text
Administrator everywhere
```

Prefer:

```text
Specific identity
      +
Specific action
      +
Specific resource
      +
Specific conditions
```

---

# Temporary Credentials

Prefer temporary credentials over long-lived static secrets where possible.

Examples:

```text
AWS STS
Azure access tokens
GCP short-lived credentials
Workload identities
```

Benefits:

```text
Expiration
Reduced credential exposure
Automatic rotation
Scoped access
```

---

# Cloud Resource Hierarchies

## AWS

```text
Organization
    |
Accounts
    |
Resources
```

## Azure

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

## GCP

```text
Organization
   |
Folders
   |
Projects
   |
Resources
```

Permissions may be inherited through these hierarchies.

---

# Compute

Cloud virtual-machine services:

```text
AWS
 |
EC2

Azure
 |
Virtual Machines

GCP
 |
Compute Engine
```

Security review includes:

```text
Public exposure
Firewall rules
Attached identity
Operating-system security
Metadata access
Secrets
Disks
```

---

# Cloud Storage

Major object-storage services:

```text
AWS
 |
S3

Azure
 |
Blob Storage

GCP
 |
Cloud Storage
```

Important security questions:

```text
Is it public?

Who can read it?

Who can write it?

Does it contain sensitive data?

Are access logs enabled?

Is encryption configured?

Can policies be modified?
```

---

# Networking

Cloud networking commonly includes:

```text
Virtual Networks
Subnets
Routes
Firewalls
Security Groups
Load Balancers
Gateways
Private Endpoints
```

Conceptually:

```text
Internet
   |
Firewall / Security Group
   |
Public Resource
   |
Private Network
   |
Internal Resource
```

---

# Secrets

Cloud applications require credentials such as:

```text
Database passwords
API keys
Certificates
Tokens
Encryption keys
```

Prefer dedicated secret-management systems.

Examples:

```text
AWS Secrets Manager
Azure Key Vault
GCP Secret Manager
```

Avoid:

```text
Secrets in Git
Secrets in Docker images
Hardcoded passwords
Public configuration files
Long-lived access keys
```

---

# Metadata Services

Cloud virtual machines often expose metadata services to workloads.

Metadata may provide:

```text
Instance information
Network information
Temporary identity credentials
```

Conceptually:

```text
VM / Workload
      |
Metadata Service
      |
Temporary Credentials
      |
Cloud API
```

Therefore:

```text
Application compromise
        +
Overprivileged workload identity
        =
Potential cloud-resource compromise
```

---

# Containers

Docker provides container runtime/build functionality.

```text
Docker
   |
Container
```

Important security areas:

```text
Runtime user
Capabilities
Host mounts
Docker socket
Secrets
Images
Cloud identity
```

---

# Kubernetes

Kubernetes orchestrates containerized workloads.

```text
Cluster
 |
 +-- Control Plane
 |
 +-- Nodes
      |
      +-- Pods
           |
           +-- Containers
```

Important security areas:

```text
RBAC
Service Accounts
Secrets
Privileged Pods
hostPath
Network Policies
Cloud IAM
```

---

# Managed Kubernetes

Major managed platforms:

```text
AWS
 |
EKS

Azure
 |
AKS

GCP
 |
GKE
```

Security becomes a combination of:

```text
Cloud IAM
    +
Kubernetes RBAC
    +
Container Security
```

---

# Serverless

Major serverless technologies include:

```text
AWS Lambda
Azure Functions
Google Cloud Functions
Cloud Run
```

Conceptually:

```text
Event
 ↓
Function
 ↓
Execution Identity
 ↓
Cloud Resources
```

The execution identity should follow least privilege.

---

# Logging

Major cloud audit/logging technologies:

```text
AWS
 |
CloudTrail

Azure
 |
Activity Log

GCP
 |
Cloud Audit Logs
```

Logs help answer:

```text
Who?
What action?
Which resource?
When?
From where?
```

---

# Cloud Enumeration Methodology

Use a structured approach:

```text
CURRENT IDENTITY
       ↓
TENANT / ACCOUNT / PROJECT
       ↓
IAM PERMISSIONS
       ↓
RESOURCE HIERARCHY
       ↓
COMPUTE
       ↓
STORAGE
       ↓
NETWORK
       ↓
SECRETS
       ↓
WORKLOAD IDENTITIES
       ↓
LOGGING
```

---

# Starting Commands

## AWS

```bash
aws sts get-caller-identity
aws configure list
```

## Azure

```bash
az account show
az account list
```

## GCP

```bash
gcloud auth list
gcloud config list
gcloud projects list
```

## Kubernetes

```bash
kubectl config current-context
kubectl auth can-i --list
```

## Docker

```bash
id
docker ps -a
docker info
```

---

# Common Cloud Misconfigurations

Look for:

```text
Overprivileged IAM
Public storage
Exposed secrets
Public administrative interfaces
Overly broad firewall rules
Long-lived credentials
Unused privileged accounts
Overprivileged workload identities
Public databases
Weak logging
Privileged containers
Overprivileged Kubernetes RBAC
```

---

# Cloud Attack-Path Mindset

Cloud security should be viewed as relationships.

Example:

```text
Application
    |
Runs as
    v
Service Identity
    |
CanRead
    v
Secret
    |
Contains
    v
Privileged Credential
    |
CanAccess
    v
Production
```

A low-risk-looking application vulnerability can therefore become a major cloud-security issue.

---

# Cross-Layer Security

Cloud environments contain multiple layers:

```text
Cloud IAM
    ↓
Virtual Machine / Cluster
    ↓
Container
    ↓
Application
    ↓
Data
```

Compromise at one layer may expose credentials belonging to another.

Example:

```text
Web Application
      ↓
Container
      ↓
Cloud Identity
      ↓
Storage Bucket
      ↓
Sensitive Data
```

---

# AWS vs Azure vs GCP

Approximate mapping:

```text
AWS                 Azure                 GCP

Account             Subscription          Project

IAM                 Entra ID + RBAC       IAM

IAM Role            Managed Identity      Service Account

EC2                 Virtual Machine       Compute Engine

S3                  Blob Storage          Cloud Storage

EKS                 AKS                   GKE

Lambda              Functions             Cloud Functions

Secrets Manager     Key Vault             Secret Manager

CloudTrail          Activity Log          Cloud Audit Logs
```

These are conceptual comparisons, not exact technical equivalents.

---

# Core Security Principles

Remember:

```text
1. Least Privilege

2. Strong Authentication

3. Temporary Credentials

4. Secret Management

5. Network Segmentation

6. Secure Workload Identities

7. Logging and Monitoring

8. Encryption

9. Patch Management

10. Regular Access Reviews
```

---

# Key Takeaway

Cloud security is primarily about:

```text
IDENTITY
    ↓
PERMISSION
    ↓
RESOURCE
    ↓
DATA
```

But modern environments add:

```text
Containers
Kubernetes
Serverless
Workload identities
Cross-account relationships
```

Therefore always ask:

```text
Who am I?

What can I do?

What identity does this workload use?

What resources can that identity access?

Can I influence another identity?

Where are the secrets?

What happens if this workload is compromised?
```

In modern cloud environments:

```text
IDENTITY IS THE NEW PERIMETER.
```
