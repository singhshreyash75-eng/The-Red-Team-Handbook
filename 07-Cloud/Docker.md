# Docker

> Docker and container-security fundamentals for cloud environments and authorized security assessments.

## What is Docker?

**Docker** is a platform for packaging and running applications inside:

```text
Containers
```

A container packages:

```text
Application
Dependencies
Libraries
Configuration
```

while normally sharing the host's kernel.

---

# Containers vs Virtual Machines

## Virtual Machine

```text
Physical Host
     |
Hypervisor
     |
+----+----+
|         |
VM1       VM2
|         |
Guest OS  Guest OS
|         |
Apps      Apps
```

Each VM normally contains its own operating-system kernel.

---

## Container

```text
Host
 |
Host Kernel
 |
Container Runtime
 |
+-----------+-----------+
|           |           |
Container1  Container2  Container3
|           |           |
App         App         App
```

Containers generally share the host kernel.

Therefore:

```text
Container != Virtual Machine
```

---

# Core Docker Concepts

Important terms:

```text
Image
Container
Dockerfile
Registry
Volume
Bind Mount
Docker Daemon
Docker Socket
Network
```

---

# Image

A Docker **image** is a packaged template.

```text
Image
 |
 +-- Application
 +-- Libraries
 +-- Dependencies
 +-- Configuration
```

List local images:

```bash
docker images
```

---

# Container

A **container** is a running instance of an image.

```text
Image
  ↓
docker run
  ↓
Container
```

List running containers:

```bash
docker ps
```

All containers:

```bash
docker ps -a
```

---

# Dockerfile

A **Dockerfile** describes how to build an image.

Example:

```dockerfile
FROM ubuntu:24.04

WORKDIR /app

COPY . .

RUN apt-get update

CMD ["./app"]
```

Important instructions:

```text
FROM
RUN
COPY
ADD
WORKDIR
ENV
USER
EXPOSE
CMD
ENTRYPOINT
```

---

# Build

Build an image:

```bash
docker build -t myapp .
```

Conceptually:

```text
Dockerfile
    ↓
docker build
    ↓
Image
```

---

# Run

Run a container:

```bash
docker run myapp
```

Interactive example:

```bash
docker run -it ubuntu /bin/bash
```

---

# Container Enumeration

Basic commands:

```bash
docker ps
docker ps -a
docker images
docker info
```

Inspect:

```bash
docker inspect <container>
```

Logs:

```bash
docker logs <container>
```

Processes:

```bash
docker top <container>
```

---

# Execute Inside a Container

In an authorized environment:

```bash
docker exec -it <container> /bin/bash
```

If Bash is unavailable:

```bash
docker exec -it <container> /bin/sh
```

---

# Docker Daemon

Docker commonly uses a privileged daemon:

```text
dockerd
```

Conceptually:

```text
Docker CLI
    |
    v
Docker Daemon
    |
    +-- Containers
    +-- Images
    +-- Networks
    +-- Volumes
```

Control over the daemon is highly security-sensitive.

---

# Docker Socket

Common Unix socket:

```text
/var/run/docker.sock
```

Check:

```bash
ls -l /var/run/docker.sock
```

Conceptually:

```text
User / Process
      |
Docker Socket
      |
      v
Docker Daemon
      |
      v
Host Resources
```

Access to a highly privileged Docker daemon can effectively represent extensive control over the host.

Therefore:

```text
docker.sock = HIGH VALUE
```

---

# Docker Group

Check:

```bash
id
groups
```

Look for:

```text
docker
```

On conventional Docker hosts, membership in the Docker group should generally be treated as highly privileged.

Do not consider it an ordinary low-privilege group.

---

# Volumes

Volumes provide persistent storage.

```text
Container
    |
    v
Volume
    |
Persistent Data
```

List:

```bash
docker volume ls
```

Inspect:

```bash
docker volume inspect <volume>
```

---

# Bind Mounts

Bind mounts map host paths into containers.

Conceptually:

```text
HOST
/etc/app
   |
   | bind mount
   v
CONTAINER
/config
```

This is security-sensitive because the container may gain access to host files.

Inspect mounts:

```bash
docker inspect <container>
```

Look for:

```text
Source
Destination
Mode
RW
```

---

# Sensitive Mounts

Particularly sensitive host paths can include:

```text
/
 /etc
 /root
 /home
 /var/run/docker.sock
```

The impact depends on:

```text
Read/write mode
Container privileges
Runtime user
Host configuration
```

---

# Docker Networks

List networks:

```bash
docker network ls
```

Inspect:

```bash
docker network inspect <network>
```

Containers can communicate through Docker networks.

Conceptually:

```text
Container A
      |
Docker Network
      |
Container B
```

Do not assume that because a service is not publicly exposed it is unreachable from other containers.

---

# Port Publishing

Example:

```bash
docker run -p 8080:80 nginx
```

Conceptually:

```text
Host :8080
     ↓
Container :80
```

Inspect:

```bash
docker ps
```

Security review should determine:

```text
Which interface?
Which host port?
Which container service?
Should it be externally reachable?
```

---

# Environment Variables

Applications frequently receive configuration through environment variables.

Inspect container configuration:

```bash
docker inspect <container>
```

Potentially sensitive variables include:

```text
DATABASE_PASSWORD
API_KEY
SECRET_KEY
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
```

Avoid using environment variables as an unmanaged secret store when stronger secret-management mechanisms are available.

---

# Image Layers

Docker images consist of:

```text
Layers
```

Conceptually:

```text
Base Image
   ↓
Layer 1
   ↓
Layer 2
   ↓
Application Layer
```

Deleting a secret in a later Dockerfile instruction does not necessarily guarantee that the secret never existed in an earlier image layer.

Therefore avoid placing secrets into image builds.

---

# Registries

Images can be stored in registries such as:

```text
Docker Hub
Amazon ECR
Azure Container Registry
Google Artifact Registry
Private Registries
```

Security considerations:

```text
Registry authentication
Private vs public images
Image provenance
Image scanning
Credentials
Tags
```

---

# Running as Root

Containers frequently default to:

```text
root
```

Check Dockerfile:

```dockerfile
USER appuser
```

Prefer a non-root runtime user where possible.

Conceptually:

```text
Container root
      !=
Automatically host root
```

but running as root inside a container increases risk if other isolation boundaries fail or excessive privileges/mounts exist.

---

# Privileged Containers

Docker supports highly privileged containers.

Conceptually:

```text
Normal Container
      |
Restricted host access
```

versus:

```text
Privileged Container
      |
Significantly expanded host access
```

Inspect configuration:

```bash
docker inspect <container>
```

Look for:

```text
Privileged
```

Privileged mode dramatically weakens container isolation and should be avoided unless genuinely required.

---

# Linux Capabilities

Linux splits some traditional root privileges into:

```text
Capabilities
```

Examples:

```text
CAP_NET_ADMIN
CAP_SYS_ADMIN
CAP_SYS_PTRACE
CAP_NET_RAW
```

Containers should receive only the capabilities they require.

Conceptually:

```text
Root privileges
      ↓
Split into capabilities
      ↓
Grant minimum required set
```

---

# CAP_SYS_ADMIN

`CAP_SYS_ADMIN` is especially broad.

It enables many privileged kernel operations.

Therefore:

```text
CAP_SYS_ADMIN
```

inside a container deserves careful security review.

---

# Namespaces

Docker isolation relies partly on Linux:

```text
Namespaces
```

Examples:

```text
PID
Network
Mount
UTS
IPC
User
```

Conceptually:

```text
Container A sees
its processes/network/mounts

Container B sees
its own isolated view
```

---

# cgroups

**Control Groups (cgroups)** control and account for resource usage.

Examples:

```text
CPU
Memory
Processes
I/O
```

Namespaces primarily help with isolation.

cgroups primarily help with resource control.

---

# Container Security Boundary

Never assume:

```text
Container
=
Perfect sandbox
```

Container security depends on:

```text
Kernel
Runtime
Capabilities
Mounts
Namespaces
User
Seccomp
AppArmor / SELinux
Daemon access
Configuration
```

---

# Cloud Credentials Inside Containers

Cloud workloads may receive credentials through:

```text
Environment variables
Mounted secrets
Metadata services
Workload identities
IAM roles
Managed identities
Service accounts
```

Therefore a compromised container may have access to cloud APIs even when it cannot escape to the host.

Conceptually:

```text
Container Compromise
       |
       +--> Application data
       |
       +--> Secrets
       |
       +--> Cloud identity
                |
                v
           Cloud Resources
```

This is extremely important in cloud security.

---

# Common Misconfigurations

Look for:

```text
Privileged containers
Docker socket mounted
Host filesystem mounts
Containers running as root
Excessive capabilities
Exposed secrets
Public management APIs
Weak registry permissions
Untrusted images
Outdated images
Sensitive environment variables
```

---

# Enumeration Checklist

```bash
id
groups

docker version
docker info

docker ps -a
docker images

docker network ls
docker volume ls
```

Inspect interesting containers:

```bash
docker inspect <container>
```

Check socket:

```bash
ls -l /var/run/docker.sock
```

---

# Security Review Flow

```text
Docker Access?
      ↓
Current User / Groups
      ↓
Containers
      ↓
Images
      ↓
Mounts
      ↓
Capabilities
      ↓
Privileged Mode
      ↓
Secrets
      ↓
Networks
      ↓
Cloud Identity
```

---

# Docker in Cloud Environments

Docker commonly appears underneath cloud/container platforms:

```text
AWS
Azure
GCP
Kubernetes
CI/CD
```

Cloud security therefore requires understanding both:

```text
Cloud IAM
    +
Container Security
```

Example:

```text
Container
    |
Cloud Role
    |
    v
S3 / Key Vault / Storage
```

A container compromise can become a cloud compromise if the workload identity is excessively privileged.

---

# Docker vs Kubernetes

Docker primarily deals with:

```text
Building
Running
Managing containers
```

Kubernetes primarily deals with:

```text
Orchestrating containers
across clusters
```

Conceptually:

```text
Docker
   ↓
Container
```

versus:

```text
Kubernetes
    ↓
Many containers
    ↓
Many nodes
    ↓
Cluster orchestration
```

---

# Defensive Hardening

Prefer:

```text
Non-root containers
Minimal images
Minimal capabilities
Read-only filesystems
No unnecessary host mounts
No Docker socket exposure
Secret managers
Image scanning
Signed/trusted images
Runtime monitoring
Network segmentation
Regular patching
```

Follow:

```text
Least Privilege
```

at every layer.

---

# Key Takeaway

Container security can be summarized as:

```text
IMAGE
  +
RUNTIME
  +
USER
  +
CAPABILITIES
  +
MOUNTS
  +
SECRETS
  +
NETWORK
  +
CLOUD IDENTITY
```

The most important security question is not simply:

```text
"Can I access the container?"
```

but:

```text
"What trust and privileges
does this container inherit?"
```

A container with access to:

```text
Docker socket
Host filesystem
Privileged capabilities
Powerful cloud identity
```

can have security impact far beyond the application running inside it.
