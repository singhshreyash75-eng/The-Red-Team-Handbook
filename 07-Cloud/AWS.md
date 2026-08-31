# AWS

> Amazon Web Services fundamentals, enumeration concepts, identity model, and common cloud-security risks for authorized labs and assessments.

## What is AWS?

**AWS = Amazon Web Services**

AWS is a cloud-computing platform that provides services such as:

```text
Virtual Machines
Storage
Databases
Networking
Identity Management
Serverless Computing
Containers
Monitoring
Secrets Management
```

Instead of owning physical infrastructure:

```text
Traditional
    |
Physical Servers
    |
Applications
```

organizations can use:

```text
AWS
 |
 +-- Compute
 +-- Storage
 +-- Networking
 +-- Identity
 +-- Databases
 +-- Security
```

---

# Core AWS Concepts

Important terms:

```text
Account
Region
Availability Zone
IAM
EC2
S3
VPC
Lambda
RDS
CloudTrail
Secrets Manager
STS
```

---

# AWS Account

An **AWS account** is a major security and billing boundary.

Resources generally exist inside an AWS account.

```text
AWS Account
    |
    +-- IAM
    +-- EC2
    +-- S3
    +-- VPC
    +-- Lambda
    +-- RDS
```

An organization may contain many AWS accounts.

---

# AWS Organizations

Large environments commonly organize accounts through:

```text
AWS Organizations
```

Conceptually:

```text
Organization
     |
     +-- Management Account
     |
     +-- Production Account
     |
     +-- Development Account
     |
     +-- Security Account
```

This provides stronger separation than putting every workload into one account.

---

# Regions

AWS infrastructure is divided into **Regions**.

Examples:

```text
us-east-1
us-west-2
eu-west-1
ap-south-1
```

A resource may exist only in a particular region.

This matters during enumeration.

Example:

```bash
aws ec2 describe-instances --region ap-south-1
```

If you query the wrong region:

```text
No resources found
```

does not necessarily mean:

```text
No resources exist.
```

---

# Availability Zones

Each region contains multiple:

```text
Availability Zones (AZs)
```

Example:

```text
Region: ap-south-1

+-- ap-south-1a
+-- ap-south-1b
+-- ap-south-1c
```

AZs provide infrastructure separation and improve availability.

---

# AWS CLI

AWS provides a command-line interface:

```text
AWS CLI
```

Check installation:

```bash
aws --version
```

Configured identities/profiles:

```bash
aws configure list
aws configure list-profiles
```

Configuration commonly exists under:

```text
~/.aws/config
~/.aws/credentials
```

These files are security-sensitive.

---

# Identify Current Principal

One of the most important AWS commands:

```bash
aws sts get-caller-identity
```

Example conceptual response:

```json
{
  "UserId": "...",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/alice"
}
```

This answers:

```text
Who am I?
Which AWS account?
What principal am I operating as?
```

Always start here when valid credentials are already available in an authorized environment.

---

# ARN

**ARN = Amazon Resource Name**

ARNs uniquely identify AWS resources.

General structure:

```text
arn:partition:service:region:account-id:resource
```

Example:

```text
arn:aws:iam::123456789012:user/alice
```

Another example:

```text
arn:aws:s3:::example-bucket
```

ARNs appear extensively in IAM policies.

---

# IAM

**IAM = Identity and Access Management**

IAM controls:

```text
Who
can perform
What action
on
Which resource
under
Which conditions
```

Important IAM entities:

```text
Users
Groups
Roles
Policies
```

---

# IAM User

An IAM user represents an identity inside an AWS account.

Example:

```text
alice
```

A user may have:

```text
Console credentials
Access keys
Group memberships
Policies
```

Modern environments increasingly prefer temporary role-based credentials over long-lived IAM-user access keys.

---

# IAM Groups

Groups collect users.

```text
Alice ----+
Bob ------+--> Developers
Charlie --+
```

Policies can then be attached to:

```text
Developers
```

rather than individually to every user.

---

# IAM Roles

Roles are extremely important in AWS.

A role is an identity that can be **assumed**.

```text
User / Service
      |
   AssumeRole
      |
      v
     Role
      |
 Temporary Credentials
```

Roles are commonly used by:

```text
EC2
Lambda
AWS services
Human administrators
Cross-account users
CI/CD systems
```

---

# STS

**STS = Security Token Service**

STS issues temporary credentials.

These generally contain:

```text
Access Key ID
Secret Access Key
Session Token
Expiration
```

Role assumption commonly relies on STS.

---

# Access Keys

Programmatic AWS credentials can include:

```text
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
```

Temporary credentials additionally include:

```text
AWS_SESSION_TOKEN
```

Environment variables may therefore contain:

```bash
env | grep AWS
```

Treat all of these values as secrets.

---

# EC2

**EC2 = Elastic Compute Cloud**

EC2 provides virtual machines.

```text
AWS
 |
 EC2
 |
 +-- Linux VM
 +-- Windows VM
 +-- Application Server
```

List instances when authorized:

```bash
aws ec2 describe-instances
```

Important properties include:

```text
Instance ID
AMI
Public IP
Private IP
Security Groups
IAM Role
Subnet
VPC
Tags
```

---

# Instance Metadata

EC2 instances can obtain information and temporary role credentials through the:

```text
Instance Metadata Service
```

Modern AWS supports:

```text
IMDSv2
```

which adds session-oriented protections.

Metadata access is security-sensitive because attached role credentials may be available to workloads on the instance.

Defensively:

```text
Prefer IMDSv2
Restrict metadata access
Apply least privilege to instance roles
```

---

# S3

**S3 = Simple Storage Service**

S3 stores objects inside buckets.

```text
S3
 |
 +-- Bucket
      |
      +-- object.txt
      +-- backup.zip
      +-- image.png
```

List accessible buckets:

```bash
aws s3 ls
```

S3 receives its own dedicated notes:

```text
S3.md
```

---

# VPC

**VPC = Virtual Private Cloud**

A VPC is a logically isolated AWS network.

```text
VPC
 |
 +-- Subnets
 |
 +-- Route Tables
 |
 +-- Security Groups
 |
 +-- Network ACLs
 |
 +-- Gateways
```

Conceptually:

```text
Internet
   |
Internet Gateway
   |
Public Subnet
   |
Private Subnet
   |
Database
```

---

# Security Groups

Security Groups act as stateful virtual firewalls around resources such as EC2 instances.

Example concept:

```text
Inbound:

TCP 443
Source: Internet
```

Security review should look for overly broad rules such as:

```text
0.0.0.0/0
```

especially on sensitive administrative services.

---

# Lambda

AWS Lambda provides:

```text
Serverless Functions
```

Conceptually:

```text
Event
  ↓
Lambda Function
  ↓
Code executes
  ↓
AWS APIs / Application
```

Security-sensitive areas include:

```text
Execution role
Environment variables
Secrets
Dependencies
Resource policies
```

---

# RDS

**RDS = Relational Database Service**

Supports managed database engines such as:

```text
PostgreSQL
MySQL
MariaDB
SQL Server
Oracle
```

Important security considerations:

```text
Public exposure
Security groups
Database credentials
Encryption
Backups/snapshots
IAM integration
```

---

# CloudTrail

**AWS CloudTrail** records many AWS API activities.

Conceptually:

```text
Principal
   |
AWS API Call
   |
   v
CloudTrail
   |
Audit Record
```

It is critical for:

```text
Detection
Incident Response
Forensics
Compliance
```

---

# Secrets

AWS provides services such as:

```text
Secrets Manager
Systems Manager Parameter Store
```

Applications should avoid hardcoding credentials in:

```text
Source code
Docker images
User data
Environment files
Git repositories
```

---

# AWS Enumeration Mindset

When authorized credentials are available:

```text
1. Who am I?
       ↓
2. Which account?
       ↓
3. Which permissions?
       ↓
4. Which regions?
       ↓
5. Which resources?
       ↓
6. Which roles?
       ↓
7. Which trust relationships?
       ↓
8. Which sensitive data?
```

Start:

```bash
aws sts get-caller-identity
```

Then investigate only resources permitted by the assessment scope.

---

# Common Security Misconfigurations

Important examples:

```text
Public S3 buckets
Overly permissive IAM
Long-lived access keys
Exposed secrets
Overly broad security groups
Excessive role permissions
Unsafe trust policies
Public databases
Weak logging
Unprotected metadata access
```

---

# Least Privilege

Avoid:

```json
{
  "Effect": "Allow",
  "Action": "*",
  "Resource": "*"
}
```

unless there is a legitimate administrative requirement.

Prefer:

```text
Required actions
       +
Required resources
       +
Required conditions
```

---

# Shared Responsibility Model

Cloud security responsibility is shared.

Conceptually:

```text
AWS
 |
Security OF the cloud
```

Customer:

```text
Security IN the cloud
```

AWS protects underlying infrastructure.

Customers remain responsible for many areas including:

```text
IAM
Data
Application security
Resource configuration
Operating systems on EC2
Network rules
Secrets
```

---

# Key Takeaway

Think of AWS security as:

```text
IDENTITY
   +
PERMISSIONS
   +
RESOURCES
   +
NETWORK
   +
DATA
   +
LOGGING
```

The first question when analyzing an AWS environment should usually be:

```bash
aws sts get-caller-identity
```

because cloud security begins with understanding:

```text
WHO AM I
and
WHAT CAN THIS IDENTITY DO?
```
