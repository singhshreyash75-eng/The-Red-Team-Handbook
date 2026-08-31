# GCP

> Google Cloud Platform fundamentals, identity, resource hierarchy, enumeration, and security notes for authorized labs and assessments.

## What is GCP?

**GCP = Google Cloud Platform**

GCP is Google's cloud-computing platform.

It provides services for:

```text
Compute
Storage
Networking
Databases
Identity
Containers
Serverless
Logging
Secrets
Machine Learning
```

Core services include:

```text
Compute Engine
Cloud Storage
Google Kubernetes Engine
Cloud Run
Cloud Functions
Cloud SQL
IAM
Secret Manager
Cloud Logging
```

---

# GCP Resource Hierarchy

One of the most important GCP concepts is its hierarchy:

```text
Organization
     |
   Folders
     |
  Projects
     |
 Resources
```

Example:

```text
Organization
 |
 +-- Production
 |      |
 |      +-- prod-project
 |
 +-- Development
        |
        +-- dev-project
```

Permissions can be inherited down this hierarchy.

---

# Organization

An **Organization** is the highest-level GCP resource.

It commonly represents a company or enterprise.

```text
Organization
      |
      +-- Folders
      +-- Projects
```

Organization-level permissions can have extremely broad impact.

---

# Folders

Folders organize projects.

Example:

```text
Organization
 |
 +-- Production
 |      |
 |      +-- Project A
 |      +-- Project B
 |
 +-- Development
        |
        +-- Project C
```

Folders can also carry IAM policies inherited by projects underneath them.

---

# Projects

A **Project** is one of the most important administrative boundaries in GCP.

Resources generally belong to projects.

```text
Project
 |
 +-- Compute Instances
 +-- Storage Buckets
 +-- Service Accounts
 +-- Databases
 +-- Networks
```

Projects also provide boundaries for:

```text
Billing
APIs
IAM
Quotas
Resources
```

---

# gcloud CLI

Google Cloud provides:

```text
gcloud
```

Check:

```bash
gcloud version
```

Current configuration:

```bash
gcloud config list
```

Authenticated accounts:

```bash
gcloud auth list
```

Projects:

```bash
gcloud projects list
```

Current project:

```bash
gcloud config get-value project
```

---

# Enumeration Starting Point

When valid credentials are available in an authorized environment:

```text
Who am I?
     ↓
Which project?
     ↓
Which organization/folder?
     ↓
Which IAM permissions?
     ↓
Which resources?
```

Useful:

```bash
gcloud auth list
gcloud config list
gcloud projects list
```

---

# IAM

GCP uses:

```text
IAM
=
Identity and Access Management
```

The basic model is:

```text
Principal
    |
    v
Role
    |
    v
Permissions
    |
    v
Resource
```

Example:

```text
Alice
  |
Storage Viewer
  |
Project
```

---

# Principals

GCP principals can include:

```text
Google Accounts
Groups
Service Accounts
Domains
Workload identities
```

---

# Roles

Roles contain collections of permissions.

Three broad categories:

```text
Basic Roles
Predefined Roles
Custom Roles
```

---

# Basic Roles

Historically common basic roles include:

```text
Owner
Editor
Viewer
```

These roles can be broad.

Prefer more granular roles where possible.

---

# Predefined Roles

Google provides service-specific roles.

Examples:

```text
roles/storage.objectViewer
roles/compute.viewer
roles/secretmanager.secretAccessor
```

These are generally preferable to broad basic roles.

---

# Custom Roles

Organizations can create:

```text
Custom Roles
```

containing only required permissions.

This can support:

```text
Least Privilege
```

---

# IAM Policy

An IAM policy connects:

```text
Principal
    +
Role
    +
Resource
```

Conceptually:

```text
user:alice@example.com
        |
roles/viewer
        |
Project A
```

---

# Policy Inheritance

Permissions can be inherited.

```text
Organization
    |
    | IAM policy
    v
Folder
    |
    v
Project
    |
    v
Resource
```

Therefore a permission visible on a project may originate from a higher level.

---

# Service Accounts

**Service Accounts** are extremely important in GCP.

They provide identities for:

```text
Applications
VMs
Cloud Functions
GKE workloads
Automation
CI/CD
```

Conceptually:

```text
Application
     |
Service Account
     |
     v
GCP APIs
```

---

# Service Account Credentials

Service accounts can authenticate using mechanisms such as:

```text
Attached workload identity
Short-lived credentials
Service account keys
Workload Identity Federation
```

Long-lived JSON service-account keys should be minimized.

---

# Service Account Key

A downloaded service-account key is highly sensitive.

Conceptually it may contain:

```text
project_id
private_key
client_email
client_id
```

Treat it as:

```text
Credential
```

Do not:

```text
Commit to Git
Bake into Docker image
Store in public bucket
Hardcode in source code
```

---

# Compute Engine

GCP virtual machines are provided by:

```text
Compute Engine
```

List instances:

```bash
gcloud compute instances list
```

Important properties include:

```text
Name
Zone
Internal IP
External IP
Service Account
Machine Type
Network
Tags
```

---

# Zones and Regions

GCP uses:

```text
Regions
Zones
```

Example:

```text
Region:
us-central1

Zones:
us-central1-a
us-central1-b
us-central1-c
```

Some resources are regional while others are zonal or global.

---

# Metadata Server

Compute Engine workloads can access a metadata service.

Conceptually:

```text
VM
 |
Metadata Server
 |
 +-- Instance information
 +-- Project information
 +-- Service-account tokens
```

This is security-sensitive.

A compromised workload may potentially use the permissions of its attached service account.

Therefore:

```text
VM compromise
      +
Powerful service account
      =
Potential cloud compromise
```

---

# Cloud Storage

GCP object storage is:

```text
Cloud Storage
```

It uses:

```text
Buckets
   |
   +-- Objects
```

List buckets accessible in the current context:

```bash
gcloud storage buckets list
```

List objects:

```bash
gcloud storage ls gs://<bucket>
```

Security review includes:

```text
Public access
IAM
Sensitive data
Signed URLs
Service-account access
```

---

# Google Kubernetes Engine

**GKE = Google Kubernetes Engine**

GKE provides managed Kubernetes.

```text
GCP
 |
 GKE
 |
 Kubernetes Cluster
 |
 +-- Nodes
 +-- Pods
 +-- Services
```

Security spans both:

```text
GCP IAM
+
Kubernetes RBAC
```

---

# Cloud Run

Cloud Run runs containerized applications without requiring direct cluster management.

Conceptually:

```text
Container Image
      ↓
Cloud Run
      ↓
HTTP Service
```

Review:

```text
Public access
Service account
Secrets
Environment variables
IAM
```

---

# Cloud Functions

Serverless functions:

```text
Event
 ↓
Cloud Function
 ↓
Code
 ↓
GCP APIs
```

Security-sensitive properties:

```text
Execution identity
Secrets
Trigger permissions
Environment variables
Network access
```

---

# Secret Manager

GCP provides:

```text
Secret Manager
```

for storing sensitive values.

Conceptually:

```text
Application
     |
Service Account
     |
     v
Secret Manager
     |
     v
Secret
```

The important question is:

```text
Who has secretAccessor?
```

---

# VPC

**VPC = Virtual Private Cloud**

GCP networking includes:

```text
VPC Networks
Subnets
Routes
Firewall Rules
Cloud NAT
Load Balancers
```

Conceptually:

```text
Internet
   |
Firewall
   |
VPC
   |
Subnet
   |
VM
```

---

# Firewall Rules

List authorized firewall configuration:

```bash
gcloud compute firewall-rules list
```

Look for:

```text
Broad source ranges
Administrative ports
Unnecessary public exposure
Overly broad target scope
```

Especially important:

```text
0.0.0.0/0
```

when combined with sensitive services.

---

# Logging

Important GCP services:

```text
Cloud Audit Logs
Cloud Logging
Cloud Monitoring
Security Command Center
```

Audit logs help answer:

```text
Who performed the action?
What API was called?
Which resource?
When?
```

---

# Common Security Risks

Look for:

```text
Overly broad IAM roles
Public buckets
Leaked service-account keys
Overprivileged service accounts
Public VM exposure
Weak firewall rules
Exposed secrets
Unsafe metadata access
Unused privileged identities
Insufficient logging
```

---

# Enumeration Methodology

```text
Identity
   ↓
Organization
   ↓
Folders
   ↓
Projects
   ↓
IAM
   ↓
Service Accounts
   ↓
Compute / Storage / GKE
   ↓
Secrets
   ↓
Network
```

Useful starting commands:

```bash
gcloud auth list
gcloud config list
gcloud projects list
gcloud compute instances list
```

---

# AWS vs Azure vs GCP

Approximate conceptual mapping:

```text
AWS                 Azure                  GCP

AWS Account         Subscription           Project
IAM                 Entra ID + RBAC        IAM
IAM Role            Managed Identity       Service Account
EC2                 Virtual Machine        Compute Engine
S3                  Blob Storage           Cloud Storage
EKS                 AKS                    GKE
Secrets Manager     Key Vault              Secret Manager
CloudTrail          Activity Log           Cloud Audit Logs
```

These are conceptual comparisons, not exact equivalents.

---

# Shared Responsibility

Google protects:

```text
Cloud infrastructure
Physical facilities
Underlying managed platform
```

Customers remain responsible for many areas including:

```text
IAM
Data
Workload configuration
Applications
Secrets
Network exposure
VM operating systems
```

Responsibility varies by service model.

---

# Key Takeaway

GCP security can be summarized as:

```text
ORGANIZATION
      ↓
FOLDER
      ↓
PROJECT
      ↓
IAM
      ↓
SERVICE ACCOUNT
      ↓
RESOURCE
```

Always ask:

```text
Who is the current principal?
Which role does it have?
Where was that role assigned?
What permissions are inherited?
Which resources can that identity reach?
```
