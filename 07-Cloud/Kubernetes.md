# Kubernetes

> Kubernetes fundamentals, architecture, RBAC, workloads, secrets, and container-cluster security for authorized labs and assessments.

## What is Kubernetes?

**Kubernetes**, commonly abbreviated:

```text
K8s
```

is a container orchestration platform.

Docker/container runtimes can run individual containers.

Kubernetes manages large numbers of containerized workloads across clusters.

```text
Containers
    ↓
Pods
    ↓
Nodes
    ↓
Kubernetes Cluster
```

---

# Why Kubernetes?

Kubernetes provides:

```text
Scheduling
Scaling
Service discovery
Load balancing
Self-healing
Configuration
Secret management
Rolling deployments
Container orchestration
```

---

# Cluster Architecture

High-level architecture:

```text
              Kubernetes Cluster
                     |
          +----------+----------+
          |                     |
    Control Plane             Nodes
          |                     |
     API Server              Kubelet
     Scheduler               Pods
     Controllers             Containers
     etcd
```

---

# Control Plane

The **Control Plane** manages the cluster.

Major components:

```text
kube-apiserver
etcd
kube-scheduler
kube-controller-manager
```

---

# API Server

The:

```text
kube-apiserver
```

is the central interface to Kubernetes.

Conceptually:

```text
kubectl
   |
   v
API Server
   |
   +--> Authentication
   +--> Authorization
   +--> Admission
   |
   v
Cluster Resources
```

Most Kubernetes operations ultimately interact with the API server.

---

# etcd

**etcd** is the cluster's distributed key-value database.

It stores critical Kubernetes state.

Conceptually:

```text
etcd
 |
 +-- Cluster configuration
 +-- Resource definitions
 +-- Secrets data
 +-- Workload state
```

Therefore:

```text
etcd = HIGH VALUE
```

Unauthorized etcd access can have severe cluster impact.

---

# Worker Nodes

Worker nodes run workloads.

```text
Node
 |
 +-- kubelet
 +-- Container Runtime
 +-- Pods
```

Examples of container runtimes include:

```text
containerd
CRI-O
```

---

# Kubelet

The **kubelet** runs on each node.

It communicates with the control plane and manages pods assigned to that node.

Conceptually:

```text
API Server
    |
    v
Kubelet
    |
    v
Pods
```

Kubelet interfaces should not be unnecessarily exposed.

---

# kubectl

The standard Kubernetes CLI is:

```text
kubectl
```

Check:

```bash
kubectl version
```

Current configuration:

```bash
kubectl config view
```

Current context:

```bash
kubectl config current-context
```

List contexts:

```bash
kubectl config get-contexts
```

---

# kubeconfig

Client authentication/configuration is commonly stored in:

```text
~/.kube/config
```

A kubeconfig may contain or reference:

```text
Cluster endpoint
Certificates
Tokens
Users
Contexts
Authentication plugins
```

Therefore kubeconfig files are security-sensitive.

---

# Basic Enumeration

Current context:

```bash
kubectl config current-context
```

Cluster information:

```bash
kubectl cluster-info
```

Namespaces:

```bash
kubectl get namespaces
```

Pods:

```bash
kubectl get pods
```

All namespaces:

```bash
kubectl get pods -A
```

Nodes:

```bash
kubectl get nodes
```

Services:

```bash
kubectl get services -A
```

---

# Namespace

A **namespace** logically separates Kubernetes resources.

Example:

```text
Cluster
 |
 +-- default
 |
 +-- production
 |
 +-- development
 |
 +-- kube-system
```

Namespaces help with:

```text
Organization
RBAC
Resource isolation
Policy
```

but are not automatically complete security boundaries.

---

# Pods

A **Pod** is Kubernetes' basic workload unit.

A pod contains one or more containers.

```text
Pod
 |
 +-- Container A
 |
 +-- Container B
```

Containers in the same pod generally share:

```text
Network namespace
Pod IP
Certain storage
Lifecycle
```

---

# Pod Enumeration

```bash
kubectl get pods
```

Detailed:

```bash
kubectl get pods -o wide
```

Describe:

```bash
kubectl describe pod <pod>
```

YAML:

```bash
kubectl get pod <pod> -o yaml
```

---

# Deployments

A **Deployment** manages replicated application pods.

```text
Deployment
    |
ReplicaSet
    |
 +-- Pod
 +-- Pod
 +-- Pod
```

List:

```bash
kubectl get deployments
```

---

# Services

Kubernetes **Services** expose groups of pods.

Common types:

```text
ClusterIP
NodePort
LoadBalancer
ExternalName
```

---

## ClusterIP

Usually reachable inside the cluster.

```text
Client Pod
    |
ClusterIP
    |
Target Pods
```

---

## NodePort

Exposes a service through a port on cluster nodes.

---

## LoadBalancer

Cloud environments may provision an external load balancer.

```text
Internet
   |
Cloud Load Balancer
   |
Kubernetes Service
   |
Pods
```

Review whether public exposure is intentional.

---

# Ingress

Ingress can route external HTTP/HTTPS traffic.

```text
Internet
   |
Ingress Controller
   |
   +--> app1.example.com
   |
   +--> app2.example.com
```

Security review includes:

```text
TLS
Authentication
Public exposure
Ingress-controller configuration
```

---

# Kubernetes Authentication

The API server can authenticate principals using mechanisms such as:

```text
Client certificates
Bearer tokens
OIDC
Cloud-provider identity
Service-account tokens
```

Authentication answers:

```text
Who are you?
```

---

# Kubernetes Authorization

Authorization answers:

```text
What can you do?
```

A common mechanism is:

```text
RBAC
=
Role-Based Access Control
```

---

# RBAC

Core RBAC objects:

```text
Role
ClusterRole
RoleBinding
ClusterRoleBinding
```

---

# Role

A `Role` defines permissions within a namespace.

Conceptually:

```text
Role
 |
 +-- get pods
 +-- list pods
 +-- read configmaps
```

---

# ClusterRole

A `ClusterRole` can define cluster-wide or reusable permissions.

```text
ClusterRole
      |
Cluster-level permissions
```

Its effective scope depends on how it is bound.

---

# RoleBinding

A `RoleBinding` assigns permissions to principals, normally within a namespace.

```text
User / ServiceAccount
         |
     RoleBinding
         |
         v
        Role
```

---

# ClusterRoleBinding

A `ClusterRoleBinding` assigns a ClusterRole cluster-wide.

```text
Principal
    |
ClusterRoleBinding
    |
ClusterRole
    |
Cluster-wide access
```

This can be highly security-sensitive.

---

# Check Your Permissions

Useful:

```bash
kubectl auth can-i --list
```

Specific check:

```bash
kubectl auth can-i get pods
```

Another example:

```bash
kubectl auth can-i list secrets
```

This is one of the most useful Kubernetes security-enumeration commands.

---

# Service Accounts

Kubernetes workloads can run using:

```text
Service Accounts
```

Conceptually:

```text
Pod
 |
Service Account
 |
RBAC
 |
Kubernetes API
```

List:

```bash
kubectl get serviceaccounts
```

All namespaces:

```bash
kubectl get serviceaccounts -A
```

---

# Service Account Security

A pod's service account can determine what the workload can do against the Kubernetes API.

Therefore:

```text
Pod Compromise
      +
Overprivileged Service Account
      =
Potential Cluster Impact
```

Apply:

```text
Least Privilege
```

---

# Secrets

Kubernetes has a resource type called:

```text
Secret
```

Used for data such as:

```text
Passwords
API tokens
TLS material
Application credentials
```

List only when authorized:

```bash
kubectl get secrets
```

Important:

Kubernetes Secrets are commonly represented as base64-encoded data in API/YAML representations.

```text
Base64 != Encryption
```

Actual protection depends on controls such as:

```text
etcd encryption at rest
RBAC
KMS integration
API access
```

---

# ConfigMaps

`ConfigMap` stores non-secret configuration.

```bash
kubectl get configmaps
```

Do not intentionally place secrets into ConfigMaps.

---

# Volumes

Pods can mount storage.

Examples:

```text
PersistentVolume
PersistentVolumeClaim
hostPath
ConfigMap
Secret
```

---

# hostPath

A `hostPath` volume maps a node filesystem path into a pod.

Conceptually:

```text
NODE
/etc
 |
hostPath
 |
 v
POD
/host/etc
```

This can weaken isolation significantly depending on permissions and the mounted path.

---

# Privileged Pods

Containers can be configured with elevated security contexts.

Security-sensitive settings include:

```text
privileged: true
runAsUser: 0
hostPID: true
hostNetwork: true
hostIPC: true
```

These should receive careful review.

---

# Security Context

Kubernetes allows workload security configuration through:

```text
securityContext
```

Relevant settings include:

```text
runAsNonRoot
runAsUser
allowPrivilegeEscalation
capabilities
readOnlyRootFilesystem
seccompProfile
```

Prefer restrictive settings.

---

# Linux Capabilities

Containers may receive Linux capabilities.

Examples:

```text
NET_ADMIN
SYS_ADMIN
SYS_PTRACE
NET_RAW
```

Avoid unnecessary capabilities.

Particularly broad capabilities such as:

```text
SYS_ADMIN
```

deserve careful scrutiny.

---

# Host Namespaces

Dangerous configurations can expose host namespaces.

Examples:

```yaml
hostNetwork: true
hostPID: true
hostIPC: true
```

Conceptually:

```text
Normal Pod
   |
Isolated namespace
```

versus:

```text
Host namespace enabled
   |
Greater visibility/control
over node resources
```

---

# Network Policies

By default, Kubernetes networking can be relatively permissive depending on the cluster/network plugin.

`NetworkPolicy` can restrict pod communication.

Conceptually:

```text
Frontend
   |
Allowed
   v
Backend

Frontend
   X
Database
```

Use segmentation to reduce unnecessary east-west access.

---

# Kubernetes in Cloud

Managed Kubernetes offerings include:

```text
AWS  -> EKS
Azure -> AKS
GCP  -> GKE
```

Therefore cluster security involves:

```text
Cloud IAM
     +
Kubernetes RBAC
     +
Container Security
     +
Network Security
```

---

# Workload Identity

Modern managed Kubernetes platforms can map pods/workloads to cloud identities.

Conceptually:

```text
Pod
 |
Kubernetes Service Account
 |
Workload Identity
 |
Cloud Role / Identity
 |
Cloud Resource
```

This is preferable to embedding static cloud credentials, but the mapped permissions must still follow least privilege.

---

# Cloud Escalation Risk

Suppose:

```text
Pod compromised
      ↓
Service account token
      ↓
Kubernetes API permissions
```

or:

```text
Pod compromised
      ↓
Cloud workload identity
      ↓
Cloud API permissions
```

Therefore a pod vulnerability can potentially become:

```text
Cluster compromise

or

Cloud-resource compromise
```

depending on identity permissions.

---

# Common Kubernetes Misconfigurations

Look for:

```text
Overprivileged RBAC
cluster-admin assigned broadly
Privileged pods
hostPath mounts
Host namespaces
Containers running as root
Excessive capabilities
Exposed API server
Exposed kubelet
Weak network segmentation
Secrets accessible broadly
Static cloud credentials
Unrestricted service accounts
```

---

# Enumeration Methodology

Start:

```bash
kubectl config current-context
kubectl cluster-info
```

Then:

```bash
kubectl auth can-i --list
```

Map:

```text
Current Identity
      ↓
Permissions
      ↓
Namespaces
      ↓
Pods
      ↓
Service Accounts
      ↓
RBAC
      ↓
Secrets / Config
      ↓
Nodes
      ↓
Cloud Identity
```

Useful authorized commands:

```bash
kubectl get namespaces
kubectl get pods -A
kubectl get services -A
kubectl get serviceaccounts -A
kubectl get roles -A
kubectl get rolebindings -A
kubectl get clusterroles
kubectl get clusterrolebindings
```

---

# Security Questions

Ask:

```text
Who am I?

What can I do?

Which namespace?

Which service account does this pod use?

Can it read secrets?

Can it create pods?

Can it modify workloads?

Can it access nodes?

Does it use hostPath?

Is it privileged?

Which cloud identity is attached?

Can workloads communicate unnecessarily?
```

---

# Docker vs Kubernetes

```text
Docker
   |
Runs containers
```

Kubernetes:

```text
Kubernetes
    |
Orchestrates workloads
    |
 +-- Pods
 +-- Nodes
 +-- Networking
 +-- RBAC
 +-- Secrets
 +-- Scaling
```

---

# Kubernetes Security Layers

Think in layers:

```text
Cloud IAM
    ↓
Cluster
    ↓
Kubernetes RBAC
    ↓
Namespace
    ↓
Pod
    ↓
Container
    ↓
Application
```

A weakness at one layer can interact with permissions at another.

---

# Defensive Hardening

Prefer:

```text
Least-privilege RBAC
Non-root containers
Restricted security contexts
Minimal capabilities
No unnecessary hostPath
No privileged workloads
NetworkPolicies
Secrets encryption
External secret managers
Workload identity
Image scanning
Admission controls
Audit logging
Regular patching
```

---

# Key Takeaway

Kubernetes security can be summarized as:

```text
IDENTITY
   +
RBAC
   +
PODS
   +
SERVICE ACCOUNTS
   +
SECRETS
   +
NODE ACCESS
   +
CLOUD IAM
```

The critical question is:

```text
"If this workload is compromised,
what identity and permissions
does the attacker inherit?"
```

Always trace:

```text
Pod
 ↓
Service Account
 ↓
RBAC
 ↓
Cluster Resources
```

and in cloud environments:

```text
Pod
 ↓
Workload Identity
 ↓
Cloud IAM
 ↓
Cloud Resources
```

That relationship is central to Kubernetes security.
