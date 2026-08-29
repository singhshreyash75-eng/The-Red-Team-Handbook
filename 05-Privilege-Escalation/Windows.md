# 🪟 Windows Privilege Escalation

> Windows Privilege Escalation involves identifying permissions, services, credentials, tokens, scheduled tasks, or vulnerable software that allow an authorized low-privileged user to obtain a higher Windows security context.

---

## Overview

A typical foothold might be:

```text
DOMAIN\alice
```

or:

```text
WORKSTATION\user
```

Potential higher privilege contexts include:

```text
Local Administrator
NT AUTHORITY\SYSTEM
```

The goal is to understand the machine before attempting escalation.

---

# Initial Workflow

```text
Initial Access
     ↓
Identity
     ↓
Groups / Privileges
     ↓
OS / Architecture
     ↓
Services
     ↓
Scheduled Tasks
     ↓
File / Registry Permissions
     ↓
Credentials
     ↓
Installed Software
     ↓
Automated Cross-Check
```

---

# 1. Current User

CMD:

```cmd
whoami
```

PowerShell:

```powershell
whoami
```

---

# 2. Groups

```cmd
whoami /groups
```

This reveals group membership.

Potentially important groups include:

```text
Administrators
Remote Desktop Users
Backup Operators
```

The actual security impact depends on policy and assigned privileges.

---

# 3. Token Privileges

```cmd
whoami /priv
```

Windows access tokens contain privileges such as:

```text
SeBackupPrivilege
SeRestorePrivilege
SeImpersonatePrivilege
SeDebugPrivilege
```

Some powerful privileges can become escalation paths depending on the system and service context.

We'll cover token abuse more deeply in the Active Directory/Windows material.

---

# 4. System Information

```cmd
systeminfo
```

Useful information includes:

```text
OS Name
OS Version
System Type
Domain/Workgroup
Hotfix information
```

PowerShell alternative:

```powershell
Get-ComputerInfo
```

---

# Architecture

```cmd
echo %PROCESSOR_ARCHITECTURE%
```

Common:

```text
AMD64
x86
ARM64
```

Architecture matters when evaluating binaries and local vulnerabilities.

---

# Hostname

```cmd
hostname
```

A hostname may suggest a role:

```text
WEB01
SQL01
BACKUP01
WS042
```

---

# Environment Variables

CMD:

```cmd
set
```

PowerShell:

```powershell
Get-ChildItem Env:
```

Useful values include:

```text
USERNAME
USERPROFILE
PATH
TEMP
ProgramFiles
```

Applications may also expose configuration values through their environment.

---

# Network Information

```cmd
ipconfig /all
```

Routes:

```cmd
route print
```

Connections:

```cmd
netstat -ano
```

PowerShell:

```powershell
Get-NetTCPConnection
```

Look for:

```text
Local-only services
Additional interfaces
Domain DNS servers
Internal routes
```

---

# Local Users

```cmd
net user
```

Specific user:

```cmd
net user USERNAME
```

PowerShell:

```powershell
Get-LocalUser
```

Availability depends on Windows version and PowerShell modules.

---

# Local Groups

```cmd
net localgroup
```

Administrators:

```cmd
net localgroup administrators
```

This identifies accounts/groups with local administrative membership.

---

# Processes

```cmd
tasklist
```

PowerShell:

```powershell
Get-Process
```

Look for:

- Security products
- Backup software
- Custom applications
- Privileged services
- Management agents

Do not terminate processes merely because they appear interesting.

---

# Services

CMD:

```cmd
sc query
```

PowerShell:

```powershell
Get-Service
```

Services are one of the most important Windows PrivEsc areas.

Investigate:

```text
Service account
Executable path
Start mode
Binary permissions
Configuration permissions
```

---

# Service Configuration

For a known service:

```cmd
sc qc SERVICE_NAME
```

This can reveal:

```text
BINARY_PATH_NAME
SERVICE_START_NAME
START_TYPE
```

A privileged service whose executable/configuration can be modified by a lower-privileged user may create a dangerous trust relationship.

---

# Unquoted Service Paths

Historically, service paths containing spaces without appropriate quoting can create executable-search ambiguity under specific conditions.

Example concept:

```text
C:\Program Files\Example App\Service.exe
```

The existence of an unquoted path alone is **not sufficient**.

Exploitability also depends on:

- Directory/file permissions
- Service privilege
- Restart capability
- Windows path-resolution behavior

Treat automated warnings as hypotheses, not proof.

---

# File Permissions

Windows ACLs determine access.

Inspect:

```cmd
icacls FILE_OR_DIRECTORY
```

Example:

```cmd
icacls "C:\Program Files\Example"
```

Look for unexpected write/modify permissions on privileged application components.

---

# Common ACL Rights

You may encounter:

```text
F  → Full Control
M  → Modify
RX → Read & Execute
R  → Read
W  → Write
```

Always interpret permissions in context.

---

# Scheduled Tasks

List:

```cmd
schtasks /query /fo LIST /v
```

PowerShell can also inspect scheduled tasks:

```powershell
Get-ScheduledTask
```

Look for:

```text
Privileged task
      ↓
Executable / Script
      ↓
Lower-privileged user can modify it
```

That trust relationship is the important part.

---

# Installed Software

PowerShell can inspect common installation metadata through registry-backed methods.

Avoid indiscriminately using legacy WMI queries that may trigger installer consistency checks.

Useful information includes:

```text
Application
Version
Install Location
```

Then research only software relevant to the assessment.

---

# PATH

Inspect:

```cmd
echo %PATH%
```

Potential problems occur when privileged software resolves executables or libraries through locations writable by lower-privileged users.

Do not assume a writable PATH entry automatically means exploitable privilege escalation.

You must identify a privileged consumer.

---

# Registry

The Windows Registry contains extensive system/application configuration.

PowerShell example:

```powershell
Get-ChildItem HKLM:\
```

Use targeted queries rather than recursively dumping the entire registry.

Potentially relevant areas include:

```text
Services
Application configuration
Startup behavior
```

---

# Credentials

Potential legitimate technical locations to investigate based on context include:

```text
Application configuration
Deployment scripts
Backup scripts
Service configuration
PowerShell history
```

Avoid indiscriminate credential harvesting.

---

# PowerShell History

Common PowerShell history is maintained by PSReadLine for many interactive users.

Rather than memorizing a hardcoded path, PowerShell can report it:

```powershell
(Get-PSReadLineOption).HistorySavePath
```

If your current account is authorized to read the file, inspect only what is necessary for the assessment.

---

# Windows Credential Manager

Windows provides Credential Manager for stored credentials.

Enumeration and use of stored credentials can be highly sensitive.

Only interact with credential stores when explicitly authorized by the engagement.

---

# AlwaysInstallElevated

Windows Installer policies have historically been associated with privilege-escalation risk when unsafe policy combinations are enabled.

Check the relevant policy configuration only in authorized environments.

The key concept:

```text
Unsafe machine policy
+
Unsafe user policy
+
Installer execution context
```

may create escalation risk.

Modern enterprise hardening should avoid enabling such insecure policies.

---

# Token Privileges

Some service accounts may possess powerful privileges.

Example:

```text
SeImpersonatePrivilege
```

This is especially relevant for service identities such as web/application service accounts.

However:

```text
Privilege Present
≠
Automatic Exploit
```

OS version, token state, available interfaces, and security mitigations matter.

---

# Local Administrators

Check:

```cmd
net localgroup administrators
```

This helps identify:

- Local admin users
- Domain groups granted local admin
- Administrative relationships

This becomes particularly important in Active Directory.

---

# Windows Defender

PowerShell administrators can inspect Defender configuration, but a low-privileged assessment should not attempt to disable or modify endpoint protection.

Treat defensive controls as part of the environment to understand, not something to tamper with unless the engagement explicitly requires it.

---

# WinPEAS

WinPEAS automates many Windows enumeration checks.

It can highlight:

```text
Services
Tasks
Permissions
Credentials/configuration
Privileges
Software
```

Detailed coverage:

```text
WinPEAS.md
```

---

# LOLBAS

LOLBAS documents legitimate Windows binaries and scripts with security-relevant functionality.

We'll cover it separately:

```text
LOLBAS.md
```

Like GTFOBins, it should be used to understand system behavior rather than blindly copy commands.

---

# Kernel / OS Vulnerabilities

Only consider OS-level exploits after checking safer configuration paths.

Workflow:

```text
Windows Version
      ↓
Build / Patch Context
      ↓
Vendor Advisory
      ↓
Exploit Preconditions
      ↓
Lab Validation
```

`systeminfo` output alone does not prove a machine is vulnerable.

---

# Manual Checklist

```text
□ whoami
□ whoami /groups
□ whoami /priv
□ hostname
□ systeminfo
□ ipconfig /all
□ route print
□ netstat -ano
□ users
□ local groups
□ processes
□ services
□ service permissions
□ scheduled tasks
□ file ACLs
□ PATH
□ installed software
□ targeted application configuration
□ credential stores/history if authorized
```

---

# Windows PrivEsc Workflow

```text
Identity
   ↓
Groups
   ↓
Token Privileges
   ↓
System Information
   ↓
Services
   ↓
Scheduled Tasks
   ↓
ACLs
   ↓
Credentials / Config
   ↓
Software
   ↓
Automated Cross-Check
   ↓
Research
   ↓
Validate
```

---

# Red Team Perspective

The key question is the same as Linux:

> **What does a privileged Windows component trust that my current user can influence?**

Examples:

```text
SYSTEM service
      ↓
Writable binary/configuration

Privileged scheduled task
      ↓
Writable script

Administrative application
      ↓
Unsafe permissions
```

Think in **trust relationships**, not exploit lists.

---

# Common Beginner Mistakes

- Running WinPEAS and blindly following colored output.
- Assuming every unquoted service path is exploitable.
- Ignoring `whoami /priv`.
- Ignoring ACLs.
- Running random local kernel exploits.
- Disabling security products unnecessarily.
- Treating Administrator and SYSTEM as identical concepts.
- Dumping credentials without explicit authorization.

---

# Interview Questions

### What is Windows Privilege Escalation?

Crossing from a lower-privileged Windows security context into a more privileged one through a vulnerability, permission, configuration, or trust relationship.

### Why run `whoami /priv`?

To inspect privileges available in the current access token.

### Why enumerate services?

Privileged services may rely on files/configuration that lower-privileged users can modify.

### What is an unquoted service path?

A potentially ambiguous executable path configuration that can become dangerous when combined with appropriate writable-path and service privilege conditions.

### Why check scheduled tasks?

Privileged tasks may execute lower-user-controlled scripts or binaries.

### What does `icacls` show?

Windows file/directory access-control permissions.

---

# Quick Revision

```text
Identity:

whoami
whoami /groups
whoami /priv

System:

hostname
systeminfo
ipconfig /all

Users:

net user
net localgroup
net localgroup administrators

Processes:

tasklist

Services:

sc query
sc qc SERVICE

Tasks:

schtasks /query /fo LIST /v

Permissions:

icacls PATH

PowerShell:

Get-Service
Get-Process
Get-ScheduledTask

Mindset:

What does SYSTEM/Admin trust
that I can influence?
```

---

# Practice

- TryHackMe Windows PrivEsc
- HTB Academy Windows Privilege Escalation
- Hack The Box
- Windows VMs you control
- Active Directory lab later

---

# References

- Microsoft Learn
- LOLBAS
- Sysinternals Documentation
- Hack The Box Academy
- TryHackMe
