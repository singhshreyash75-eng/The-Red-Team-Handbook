# WinPEAS

> Windows privilege-escalation enumeration notes for authorized labs and security assessments.

## What is WinPEAS?

**WinPEAS** is the Windows enumeration component of the PEASS-ng project.

It automates checks for Windows configurations that may be relevant to privilege escalation.

Think of:

```text
Linux   -> LinPEAS
Windows -> WinPEAS
```

Its primary role is **enumeration and discovery**.

## What WinPEAS Checks

Typical areas include:

```text
System information
Users
Groups
Privileges
Services
Scheduled tasks
Registry settings
Installed software
Processes
Network configuration
Credentials
Environment variables
Writable paths
Security products
Interesting files
```

## Manual Enumeration First

Understand the checks WinPEAS automates.

### Current User

```cmd
whoami
```

Detailed privileges:

```cmd
whoami /priv
```

Groups:

```cmd
whoami /groups
```

## System Information

```cmd
systeminfo
hostname
```

PowerShell:

```powershell
Get-ComputerInfo
```

## Users

```cmd
net user
```

Current user:

```cmd
net user %USERNAME%
```

Local groups:

```cmd
net localgroup
```

Administrators:

```cmd
net localgroup administrators
```

## Processes

```cmd
tasklist
```

PowerShell:

```powershell
Get-Process
```

## Services

CMD:

```cmd
sc query
```

PowerShell:

```powershell
Get-Service
```

Detailed service information:

```cmd
sc qc <service>
```

Security-relevant questions:

```text
Which account runs the service?
Where is the executable?
Can the current user modify it?
Can its configuration be changed?
Are parent directories writable?
```

## Scheduled Tasks

```cmd
schtasks /query /fo LIST /v
```

PowerShell:

```powershell
Get-ScheduledTask
```

Check:

```text
Task user
Executable
Arguments
Trigger
File permissions
Directory permissions
```

## Network Enumeration

```cmd
ipconfig /all
route print
arp -a
netstat -ano
```

PowerShell:

```powershell
Get-NetTCPConnection
```

## Environment

CMD:

```cmd
set
```

PowerShell:

```powershell
Get-ChildItem Env:
```

## Installed Software

PowerShell examples:

```powershell
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*
```

Also inspect:

```text
C:\Program Files\
C:\Program Files (x86)\
```

## Typical Authorized-Lab Usage

If `winPEAS.exe` is already available on the authorized system:

```cmd
winPEAS.exe
```

Output can be redirected for later review:

```cmd
winPEAS.exe > winpeas-output.txt
```

## Reading WinPEAS Results

Prioritize findings related to actual privilege boundaries.

High-value categories include:

```text
Service misconfigurations
Scheduled tasks
Interesting privileges
Writable privileged paths
Credentials
Registry configuration
Installer policies
Sensitive files
Security product configuration
```

## Windows Privileges

Always inspect:

```cmd
whoami /priv
```

Privileges can significantly change the security context of an account.

Examples WinPEAS may highlight include:

```text
SeImpersonatePrivilege
SeBackupPrivilege
SeRestorePrivilege
SeDebugPrivilege
SeTakeOwnershipPrivilege
```

The presence of a privilege does not by itself prove a viable escalation path.

## Service Analysis

Suppose WinPEAS highlights a service.

Verify manually:

```cmd
sc qc <service>
```

Then examine:

```text
SERVICE_START_NAME
BINARY_PATH_NAME
START_TYPE
```

Check filesystem permissions on referenced paths.

PowerShell can help inspect ACLs:

```powershell
Get-Acl "C:\path\to\file.exe" | Format-List
```

## Scheduled Task Analysis

If an interesting scheduled task appears:

```cmd
schtasks /query /tn "<task>" /fo LIST /v
```

Determine:

```text
Who executes it?
What does it execute?
When does it execute?
Can the referenced file be modified?
Can its directory be modified?
```

## Credential Findings

WinPEAS may identify potentially sensitive information in:

```text
configuration files
registry locations
environment variables
application files
PowerShell history
backup files
scripts
```

Treat automated credential findings as leads requiring verification.

## Security Products

WinPEAS may identify:

```text
Microsoft Defender
Antivirus
EDR
AppLocker
Firewall
PowerShell protections
```

These findings are important for understanding the system's defensive configuration.

## False Positives

As with LinPEAS:

```text
Highlighted finding != Confirmed vulnerability
```

Automated enumeration deliberately produces many leads.

Verify each relevant finding manually.

## Recommended Workflow

```text
Manual enumeration
       ↓
Run WinPEAS
       ↓
Review output
       ↓
Prioritize privilege-related findings
       ↓
Verify manually
       ↓
Research configuration
       ↓
Confirmed security finding
```

## Quick Manual Checklist

```cmd
whoami
whoami /priv
whoami /groups
systeminfo
net user
net localgroup administrators
tasklist
sc query
schtasks /query /fo LIST /v
ipconfig /all
netstat -ano
```

## LinPEAS vs WinPEAS

```text
              PEASS-ng
                 |
        ---------------------
        |                   |
     LinPEAS             WinPEAS
        |                   |
      Linux              Windows
        |                   |
SUID / sudo / cron    services / tasks
capabilities          privileges / registry
services              Windows configuration
```

## Key Takeaway

WinPEAS is best treated as an **enumeration accelerator**:

```text
Automated detection
        ↓
Potential issue
        ↓
Manual verification
        ↓
Understand security boundary
        ↓
Confirmed finding
```

Use the tool to reduce enumeration time—not as a substitute for understanding Windows permissions, services, privileges, and configuration.
