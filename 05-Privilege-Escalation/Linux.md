# 🐧 Linux Privilege Escalation

> Linux Privilege Escalation involves identifying permissions, configurations, credentials, or vulnerable components that allow an authorized low-privileged user to obtain a higher security context.

---

## Initial Mindset

Suppose your foothold is:

```bash
whoami
```

```text
www-data
```

Don't immediately search for a kernel exploit.

First ask:

```text
Who am I?
What can I execute?
What groups am I in?
What runs as root?
What files can I modify?
What secrets can I legitimately access?
```

---

## 1. User Context

```bash
whoami
```

Detailed identity:

```bash
id
```

Example:

```text
uid=1001(alice)
gid=1001(alice)
groups=1001(alice),27(sudo)
```

Group membership can be extremely important.

---

## 2. Users

```bash
cat /etc/passwd
```

Typical entry:

```text
alice:x:1001:1001:Alice:/home/alice:/bin/bash
```

Important fields include:

```text
Username
UID
GID
Home directory
Login shell
```

---

## UID 0

Traditionally:

```text
UID 0 = root-level identity
```

Check for unexpected UID 0 accounts:

```bash
awk -F: '$3 == 0 {print $1}' /etc/passwd
```

Normally this should primarily identify `root`, though legitimate system designs can vary.

---

## 3. Groups

```bash
groups
```

or:

```bash
id
```

Interesting groups may include:

```text
sudo
docker
lxd
disk
adm
```

Why?

Groups can grant capabilities beyond ordinary file access.

Group membership should be evaluated in context rather than treated automatically as exploitation.

---

## 4. OS Information

```bash
cat /etc/os-release
```

Kernel:

```bash
uname -a
```

Architecture:

```bash
uname -m
```

Record:

```text
Distribution
Version
Kernel
Architecture
```

This becomes useful if later researching local vulnerabilities.

---

## 5. Hostname

```bash
hostname
```

Hostnames may indicate machine roles:

```text
web01
db01
backup01
dev01
```

---

## 6. Environment Variables

```bash
env
```

or:

```bash
printenv
```

Pay attention to:

```text
PATH
HOME
USER
SHELL
```

Applications sometimes also place credentials or tokens in environment variables, though secrets should not be stored there unnecessarily.

---

# `sudo`

One of the highest-value checks:

```bash
sudo -l
```

This lists commands the current user is permitted to execute through `sudo`, subject to policy and authentication.

Example concept:

```text
User alice may run:
/usr/bin/some-program
```

The security question becomes:

> Can this permitted program be used in a way that crosses the intended privilege boundary?

---

## sudo Misconfigurations

Potentially dangerous policies include:

- Excessively broad commands
- Scripts writable by lower-privileged users
- Unsafe environment preservation
- Programs with shell/file execution functionality

Do not assume every sudo entry is exploitable.

Understand the exact command and constraints.

---

## GTFOBins

For Unix binaries, GTFOBins documents security-relevant behavior under configurations such as:

```text
sudo
SUID
Capabilities
```

We'll cover it separately in:

```text
GTFOBins.md
```

Use it to understand behavior, not as a substitute for reasoning.

---

# SUID

Check:

```bash
find / -perm -4000 -type f 2>/dev/null
```

SUID causes an executable to run with the effective UID of its file owner.

Typical legitimate SUID binaries exist on normal Linux systems.

Therefore:

```text
SUID Found
≠
Vulnerability
```

Look for:

- Unusual custom binaries
- Unsafe behavior
- Unexpected SUID programs

Detailed coverage:

```text
SUID.md
```

---

# SGID

Find SGID files:

```bash
find / -perm -2000 -type f 2>/dev/null
```

SGID can cause execution with the file's group privileges.

Again, presence alone is not a vulnerability.

---

# Linux Capabilities

List file capabilities:

```bash
getcap -r / 2>/dev/null
```

Capabilities divide traditional root privileges into smaller units.

Examples include:

```text
CAP_NET_RAW
CAP_NET_BIND_SERVICE
CAP_SETUID
```

Some capability assignments can create dangerous privilege boundaries.

Detailed coverage:

```text
Capabilities.md
```

---

# Writable Files

Find writable files/directories carefully.

Example:

```bash
find / -writable 2>/dev/null
```

This can produce huge output.

More useful questions:

```text
Can I modify something executed by root?
Can I modify service configuration?
Can I modify a scheduled script?
```

Writable is interesting only when paired with **privileged consumption**.

---

# File Ownership

Inspect:

```bash
ls -la FILE
```

Example:

```text
-rwxrwxr-x root developers script.sh
```

If a privileged process executes a file writable by a lower-privileged user/group, investigate why.

---

# Cron Jobs

System cron locations include:

```text
/etc/crontab
/etc/cron.d/
/etc/cron.daily/
/etc/cron.hourly/
```

View:

```bash
cat /etc/crontab
```

Look for:

```text
Root-executed jobs
Writable scripts
Unsafe paths
Custom commands
```

Detailed coverage:

```text
Cron-Jobs.md
```

---

# Processes

```bash
ps aux
```

Look for:

```text
Root processes
Custom applications
Backup tools
Monitoring agents
Database services
Scripts
```

A privileged process may consume files or configuration you can modify.

---

# Services

On systemd systems:

```bash
systemctl list-units --type=service
```

Inspect a service:

```bash
systemctl status SERVICE
```

Look for custom services and their:

```text
Executable
Configuration
Permissions
Environment
```

Detailed coverage:

```text
Services.md
```

---

# Network Services

```bash
ss -tulnp
```

A local-only service may not have been visible during external enumeration.

Example:

```text
127.0.0.1:PORT
```

Such services can reveal:

- Databases
- Admin panels
- Internal APIs
- Monitoring systems

Finding one does not automatically imply privilege escalation, but it expands local attack-surface understanding.

---

# Shell History

Where legitimately accessible:

```bash
history
```

or shell-history files may contain useful administrative context.

However, avoid collecting unrelated personal commands/data.

Secrets should never be intentionally stored in shell history.

---

# Configuration Files

Applications often use files such as:

```text
.env
config.php
settings.py
application.yml
database.yml
```

Search should be targeted based on known applications rather than recursively grepping the entire filesystem for "password".

Ask:

```text
Which application am I investigating?
Where does it store configuration?
Which files can my current user legitimately read?
```

---

# Home Directories

```bash
ls -la /home
```

Only inspect data your current security context is authorized to access.

Useful technical artifacts may include:

```text
SSH configuration
Scripts
Application config
Deployment files
```

---

# SSH Material

Typical user SSH directory:

```text
~/.ssh/
```

Potential files:

```text
authorized_keys
known_hosts
config
```

Private keys should be treated as sensitive credentials.

Finding one does not authorize reuse against arbitrary systems.

---

# Mounts

```bash
mount
```

or:

```bash
findmnt
```

Potentially interesting:

```text
NFS
Shared storage
Docker mounts
Backup volumes
```

Mount configuration can reveal system relationships.

---

# Filesystems

```bash
df -h
```

Block devices:

```bash
lsblk
```

These commands help understand storage layout.

---

# Docker

Check:

```bash
docker ps
```

and:

```bash
groups
```

Membership in a privileged container-management group can represent a significant trust boundary because container runtimes may control host resources.

Detailed coverage:

```text
Docker.md
```

---

# Scheduled Execution Monitoring

Sometimes a privileged script runs periodically but isn't obvious from static configuration.

Tools such as `pspy` can help observe process execution in authorized labs without requiring root.

Automated tools should supplement manual enumeration.

---

# LinPEAS

LinPEAS automates many Linux enumeration checks.

It can highlight:

- Sudo
- SUID
- Capabilities
- Cron
- Services
- Credentials/configuration
- Containers

Detailed coverage:

```text
LinPEAS.md
```

---

# Kernel Vulnerabilities

Only investigate kernel exploitation after safer configuration paths.

Workflow:

```text
uname -a
   ↓
Distribution / Kernel Context
   ↓
Vendor Advisory
   ↓
Patch / Backport Status
   ↓
Exploit Preconditions
   ↓
Lab Validation
```

Kernel version alone is insufficient.

---

# Manual Enumeration Checklist

```text
□ whoami
□ id
□ groups
□ /etc/passwd
□ sudo -l
□ OS/version
□ kernel
□ environment
□ SUID
□ SGID
□ capabilities
□ cron
□ processes
□ services
□ listening ports
□ application configs
□ mounts
□ container groups/runtime
□ targeted credential locations
```

---

# Practical Workflow

```text
Initial Shell
    ↓
Identity
    ↓
Groups
    ↓
sudo
    ↓
OS / Kernel
    ↓
SUID / Capabilities
    ↓
Cron / Services
    ↓
Processes
    ↓
Local Ports
    ↓
Application Configuration
    ↓
Mounts / Containers
    ↓
Automated Cross-Check
    ↓
Research
    ↓
Validate Safely
```

---

# Red Team Perspective

The core Linux PrivEsc question is:

> **What does root trust that I can influence?**

Examples:

```text
Root executes writable script
Privileged program loads attacker-controlled configuration
Overly broad sudo permission
Dangerous capability assignment
Privileged container access
```

Once you start thinking in terms of **trust relationships**, PrivEsc becomes much easier to reason about.

---

# Common Beginner Mistakes

- Running LinPEAS and ignoring manual checks.
- Immediately searching kernel exploits.
- Assuming every SUID file is exploitable.
- Ignoring `sudo -l`.
- Ignoring groups.
- Recursively searching all files for credentials without a reason.
- Missing localhost-only services.
- Failing to understand why a discovered path crosses privileges.

---

# Interview Questions

### What is Linux Privilege Escalation?

Moving from a lower-privileged Linux security context to a higher one by abusing a vulnerability, configuration, permission, or trust relationship.

### What is the first command you run?

There is no single mandatory command, but `id` is extremely useful because it shows the current UID, GID, and groups.

### Why is `sudo -l` important?

It identifies commands the current account may execute under sudo policy.

### Why check SUID?

SUID executables run with the effective UID of their owner and may create escalation paths if unsafe.

### Why check capabilities?

Capabilities can grant specific privileged operations without full root.

### Why check localhost ports?

Internal services may be inaccessible externally but reachable after obtaining a local foothold.

---

# Quick Revision

```text
Identity:

whoami
id
groups

System:

hostname
cat /etc/os-release
uname -a

High Value:

sudo -l
find / -perm -4000 -type f 2>/dev/null
getcap -r / 2>/dev/null
cat /etc/crontab
ps aux
ss -tulnp
findmnt

Mindset:

What does root trust
that I can influence?
```

---

# Practice

- TryHackMe Linux PrivEsc
- HTB Academy Linux Privilege Escalation
- Hack The Box
- VulnHub
- Linux VMs you control

---

# References

- GTFOBins
- Linux man pages
- systemd Documentation
- Hack The Box Academy
- TryHackMe
