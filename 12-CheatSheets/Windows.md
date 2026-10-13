
# Windows — Cheat Sheet

> Windows CMD, PowerShell, system enumeration, networking, permissions, services, and security auditing.

## 1. System Information

CMD:

```cmd
systeminfo
hostname
ver
```

PowerShell:

```powershell
Get-ComputerInfo
```

OS information:

```powershell
Get-CimInstance Win32_OperatingSystem
```

---

## 2. Current Identity

```cmd
whoami
whoami /all
whoami /groups
whoami /priv
```

Environment:

```cmd
echo %USERNAME%
echo %USERDOMAIN%
echo %COMPUTERNAME%
```

PowerShell:

```powershell
$env:USERNAME
$env:USERDOMAIN
$env:COMPUTERNAME
```

---

## 3. File Navigation

CMD:

```cmd
dir
cd
cd ..
type file.txt
```

PowerShell:

```powershell
Get-Location
Get-ChildItem
Set-Location C:\Users
Get-Content file.txt
```

Common aliases:

```text
pwd
ls
cd
cat
```

---

## 4. File Operations

PowerShell:

```powershell
New-Item file.txt -ItemType File
New-Item folder -ItemType Directory

Copy-Item file.txt copy.txt
Move-Item old.txt new.txt
Remove-Item file.txt
```

Recursive listing:

```powershell
Get-ChildItem C:\Temp -Recurse
```

---

## 5. Users

Local users:

```cmd
net user
```

Specific user:

```cmd
net user alice
```

PowerShell:

```powershell
Get-LocalUser
```

---

## 6. Groups

Local groups:

```cmd
net localgroup
```

Administrators:

```cmd
net localgroup Administrators
```

PowerShell:

```powershell
Get-LocalGroup
```

Group members:

```powershell
Get-LocalGroupMember Administrators
```

---

## 7. Domain Information

```cmd
echo %USERDOMAIN%
echo %LOGONSERVER%
```

Domain Controller discovery:

```cmd
nltest /dclist:corp.example
```

With the AD PowerShell module:

```powershell
Get-ADDomain
Get-ADForest
```

---

## 8. Processes

CMD:

```cmd
tasklist
```

PowerShell:

```powershell
Get-Process
```

Specific process:

```powershell
Get-Process -Name explorer
```

Process details:

```powershell
Get-CimInstance Win32_Process |
Select-Object ProcessId, ParentProcessId, Name, CommandLine
```

---

## 9. Services

CMD:

```cmd
sc query
```

Service configuration:

```cmd
sc qc <ServiceName>
```

PowerShell:

```powershell
Get-Service
```

Running services:

```powershell
Get-Service |
Where-Object Status -eq "Running"
```

---

## 10. Scheduled Tasks

CMD:

```cmd
schtasks /query /fo LIST /v
```

PowerShell:

```powershell
Get-ScheduledTask
```

Review:

```text
Executing account
Run level
Executable
Arguments
Schedule
File permissions
```

---

## 11. Network Configuration

CMD:

```cmd
ipconfig
ipconfig /all
```

PowerShell:

```powershell
Get-NetIPAddress
Get-NetIPConfiguration
```

Routes:

```cmd
route print
```

PowerShell:

```powershell
Get-NetRoute
```

---

## 12. Network Connections

CMD:

```cmd
netstat -ano
```

PowerShell:

```powershell
Get-NetTCPConnection
```

Listening sockets:

```powershell
Get-NetTCPConnection -State Listen
```

---

## 13. DNS

CMD:

```cmd
nslookup example.com
```

PowerShell:

```powershell
Resolve-DnsName example.com
```

MX records:

```powershell
Resolve-DnsName example.com -Type MX
```

---

## 14. Connectivity

Ping:

```cmd
ping 192.0.2.10
```

PowerShell:

```powershell
Test-NetConnection example.com -Port 443
```

Trace route:

```cmd
tracert example.com
```

---

## 15. Firewall

Check profiles:

```powershell
Get-NetFirewallProfile
```

List firewall rules:

```powershell
Get-NetFirewallRule
```

Enabled rules:

```powershell
Get-NetFirewallRule -Enabled True
```

Review unexpected inbound exposure.

---

## 16. File Permissions

CMD:

```cmd
icacls "C:\Temp"
```

Specific file:

```cmd
icacls "C:\Temp\file.txt"
```

PowerShell:

```powershell
Get-Acl "C:\Temp\file.txt" | Format-List
```

---

## 17. Windows ACL Concepts

Important permissions:

```text
Read
Write
Modify
Execute
Full Control
Change Permissions
Take Ownership
```

Permissions can be inherited from parent directories.

Review both explicit and inherited ACEs.

---

## 18. Registry

Query:

```cmd
reg query HKLM\SOFTWARE
```

PowerShell:

```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion"
```

The registry stores important system and application configuration.

Do not modify production registry keys without authorization and rollback planning.

---

## 19. Installed Software

PowerShell:

```powershell
Get-ItemProperty `
"HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" |
Select-Object DisplayName, DisplayVersion
```

Also review 32-bit application registry paths where relevant.

---

## 20. Updates

```powershell
Get-HotFix
```

System version:

```powershell
Get-ComputerInfo
```

Confirm security patch status against official Microsoft advisories.

---

## 21. Event Logs

Available logs:

```powershell
Get-WinEvent -ListLog *
```

Recent System events:

```powershell
Get-WinEvent -LogName System -MaxEvents 20
```

Recent Security events, with appropriate privileges:

```powershell
Get-WinEvent -LogName Security -MaxEvents 20
```

---

## 22. Important Event IDs

| Event ID | Meaning |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4634 | Logoff |
| 4672 | Special privileges assigned |
| 4688 | Process creation |
| 4720 | User created |
| 4728 | Added to global security group |
| 4732 | Added to local security group |
| 4740 | Account lockout |
| 4768 | Kerberos TGT requested |
| 4769 | Kerberos service ticket requested |
| 4771 | Kerberos preauthentication failed |

Event availability depends on audit policy.

---

## 23. PowerShell Basics

List commands:

```powershell
Get-Command
```

Help:

```powershell
Get-Help Get-Process
```

Object properties:

```powershell
Get-Process | Get-Member
```

Filter:

```powershell
Get-Service |
Where-Object Status -eq "Running"
```

---

## 24. PowerShell Execution Policy

Check:

```powershell
Get-ExecutionPolicy -List
```

Execution policy is not a comprehensive security boundary.

Also review:

```text
Application control
Constrained Language Mode
Script signing
AMSI
PowerShell logging
```

---

## 25. Windows Defender

Status:

```powershell
Get-MpComputerStatus
```

Preferences:

```powershell
Get-MpPreference
```

Check whether real-time protection and relevant security features are enabled.

---

## 26. BitLocker

Status:

```powershell
Get-BitLockerVolume
```

CMD alternative:

```cmd
manage-bde -status
```

BitLocker protects supported volumes through encryption at rest.

---

## 27. SMB Shares

List shares:

```cmd
net share
```

PowerShell:

```powershell
Get-SmbShare
```

Review:

```text
Share permissions
NTFS permissions
Guest access
SMB signing
Protocol versions
```

---

## 28. Remote Administration

Common technologies:

```text
RDP
WinRM
PowerShell Remoting
SMB
WMI
```

Relevant ports:

```text
3389  RDP
5985  WinRM HTTP
5986  WinRM HTTPS
445   SMB
```

Restrict access using network segmentation and least privilege.

---

## 29. Environment Variables

CMD:

```cmd
set
```

PowerShell:

```powershell
Get-ChildItem Env:
```

PATH:

```powershell
$env:PATH
```

Environment variables can contain sensitive configuration.

---

## 30. File Hashes

```powershell
Get-FileHash file.txt -Algorithm SHA256
```

Useful for:

```text
Integrity checks
File identification
Incident response
```

---

## 31. Windows Security Review Workflow

```text
Current Identity
      ↓
System Information
      ↓
Local Users / Groups
      ↓
Privileges
      ↓
Processes
      ↓
Services
      ↓
Scheduled Tasks
      ↓
Network Connections
      ↓
File ACLs
      ↓
Security Logs
      ↓
Patch Status
      ↓
Findings
```

---

## Key Takeaway

Windows security assessment focuses on:

```text
IDENTITIES
    +
PRIVILEGES
    +
ACLs
    +
SERVICES
    +
SCHEDULED TASKS
    +
ACTIVE DIRECTORY
    +
EVENT LOGS
```

**Always distinguish administrative access, effective permissions, and actual security impact.**
