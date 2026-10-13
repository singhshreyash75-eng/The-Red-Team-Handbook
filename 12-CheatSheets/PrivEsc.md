
# PrivEsc — Privilege Escalation Cheat Sheet

> Linux and Windows privilege-escalation enumeration, common misconfigurations, and security assessment methodology.

## 1. What is Privilege Escalation?

**Privilege Escalation (PrivEsc)** occurs when a user or process obtains permissions beyond those originally assigned.

Two categories:

```text
Vertical PrivEsc
    |
Low Privilege
    ↓
Administrator / root
```

```text
Horizontal PrivEsc
    |
User A
    ↓
Access User B's Resources
```

Privilege escalation often involves:

- Incorrect file permissions
- Excessive administrative rights
- Vulnerable software
- Misconfigured services
- Insecure scheduled tasks
- Exposed credentials

---

## 2. Linux — Initial Enumeration

Current identity:

```bash
whoami
id
groups
```

System information:

```bash
uname -a
cat /etc/os-release
hostname
```

Environment:

```bash
env
echo "$PATH"
```

Current processes:

```bash
ps aux
```

Network listeners:

```bash
ss -tuln
```

---

## 3. sudo Permissions

Check permitted commands:

```bash
sudo -l
```

Important findings:

```text
NOPASSWD entries
Unexpected administrative commands
Overly broad sudo rules
Privileged scripts writable by other users
```

Review configuration as an authorized administrator:

```bash
sudo visudo -c
```

A sudo entry is not automatically exploitable. Its security depends on the executable, arguments, and filesystem permissions.

---

## 4. SUID / SGID

**SUID** allows an executable to run with its owner's effective UID.

Find SUID files:

```bash
find /usr /bin /sbin -type f -perm -4000 2>/dev/null
```

Find SGID files:

```bash
find /usr /bin /sbin -type f -perm -2000 2>/dev/null
```

Review:

```text
Unexpected executable
    ↓
Owner
    ↓
Permissions
    ↓
Purpose
    ↓
Patch status
```

---

## 5. Linux Capabilities

Capabilities divide traditional root privileges into more granular permissions.

Search common executable locations:

```bash
getcap -r /usr/bin /usr/local/bin 2>/dev/null
```

Important examples:

```text
CAP_SETUID
CAP_SETGID
CAP_DAC_OVERRIDE
CAP_SYS_ADMIN
CAP_NET_ADMIN
```

Review unexpected capabilities assigned to user-controlled executables.

---

## 6. Writable Files and Directories

Find world-writable directories:

```bash
find /tmp /var/tmp /opt -type d -perm -0002 2>/dev/null
```

Find world-writable files:

```bash
find /opt /usr/local -type f -perm -0002 2>/dev/null
```

Security question:

```text
Can a low-privileged user modify
a file executed by a privileged service?
```

Do not modify production files merely to demonstrate a finding.

---

## 7. Cron Jobs

Current user:

```bash
crontab -l
```

System-wide:

```bash
cat /etc/crontab
ls -la /etc/cron.*
```

Review:

```text
Scheduled script
Executing identity
File ownership
Write permissions
PATH
Working directory
```

Misconfigured privileged cron jobs can create privilege boundaries that require remediation.

---

## 8. Running Services

```bash
systemctl list-units --type=service
```

Specific service:

```bash
systemctl status <service>
```

Inspect a service definition:

```bash
systemctl cat <service>
```

Check:

```text
User=
Group=
ExecStart=
WorkingDirectory=
Environment=
```

Important question:

```text
Does a privileged service depend
on lower-trust writable resources?
```

---

## 9. PATH Security

View:

```bash
echo "$PATH"
```

Review writable directories:

```bash
ls -ld /usr/local/bin /usr/local/sbin
```

Security issue:

```text
Privileged Program
       ↓
Uses Relative Executable Name
       ↓
Searches PATH
       ↓
Potentially Untrusted Directory
```

Defensive recommendation: use trusted absolute executable paths and restrictive directory permissions.

---

## 10. Kernel and Package Versions

```bash
uname -r
```

Debian/Ubuntu:

```bash
dpkg -l
```

RHEL/Fedora:

```bash
rpm -qa
```

Workflow:

```text
Kernel / Package Version
       ↓
Vendor Advisory
       ↓
Affected Version?
       ↓
Patch Status
```

Do not assume a version string proves vulnerability because distributions may backport fixes.

---

## 11. Credentials and Secrets

Review approved application configuration for:

```text
Hardcoded passwords
API keys
Service credentials
Unprotected private keys
Database connection strings
```

Sensitive material should be handled according to the engagement's authorization and retention rules.

Avoid printing or copying unnecessary secrets.

---

## 12. Docker Privileges

Inspect Docker availability:

```bash
docker info
```

Current groups:

```bash
groups
```

Containers:

```bash
docker ps
```

Important risk areas:

```text
Docker daemon access
Privileged containers
Host filesystem mounts
Docker socket exposure
Excessive Linux capabilities
```

Access to a rootful Docker daemon can effectively grant root-level control of its host.

---

## 13. Windows — Initial Enumeration

Current identity:

```cmd
whoami
whoami /all
```

System information:

```cmd
systeminfo
```

Hostname:

```cmd
hostname
```

Current privileges:

```cmd
whoami /priv
```

Groups:

```cmd
whoami /groups
```

---

## 14. Windows Services

List services:

```cmd
sc query
```

Inspect configuration:

```cmd
sc qc <ServiceName>
```

PowerShell:

```powershell
Get-CimInstance Win32_Service |
Select-Object Name, StartName, PathName, State
```

Review:

```text
Service account
Executable path
Binary permissions
Service configuration permissions
Quoted paths
```

---

## 15. Service Binary Permissions

Windows ACL inspection:

```cmd
icacls "C:\Path\service.exe"
```

Check whether non-administrative identities can modify binaries executed by privileged services.

Do not replace service binaries during a production assessment without explicit authorization.

---

## 16. Scheduled Tasks

List:

```cmd
schtasks /query /fo LIST /v
```

PowerShell:

```powershell
Get-ScheduledTask
```

Review:

```text
Task principal
Run level
Executable path
Arguments
Script permissions
```

---

## 17. Windows Privileges

```cmd
whoami /priv
```

Important examples:

```text
SeImpersonatePrivilege
SeBackupPrivilege
SeRestorePrivilege
SeDebugPrivilege
SeTakeOwnershipPrivilege
```

The presence, enablement, and actual usability of privileges are different concepts.

---

## 18. Windows File Permissions

```cmd
icacls "C:\Program Files"
```

Specific file:

```cmd
icacls "C:\Path\file.exe"
```

PowerShell:

```powershell
Get-Acl "C:\Path\file.exe" | Format-List
```

Look for unexpected write access granted to low-privileged identities.

---

## 19. Windows Security Updates

Installed hotfixes:

```powershell
Get-HotFix
```

System details:

```powershell
Get-ComputerInfo
```

Verify missing patches against Microsoft security advisories.

---

## 20. Automated Enumeration

Common authorized assessment tools:

```text
LinPEAS
WinPEAS
Seatbelt
Linux system-audit utilities
```

Important:

```text
Automated Finding
       !=
Confirmed PrivEsc
```

Review the evidence and actual permission boundary.

---

## 21. PrivEsc Assessment Workflow

```text
Initial Identity
      ↓
OS / Patch Level
      ↓
User Privileges
      ↓
Services / Processes
      ↓
File Permissions
      ↓
Scheduled Tasks
      ↓
Credentials / Secrets
      ↓
Privilege Relationships
      ↓
Manual Validation
      ↓
Remediation
```

---

## 22. Defensive Checklist

```text
[ ] Apply security patches
[ ] Restrict sudo permissions
[ ] Audit SUID/SGID
[ ] Review Linux capabilities
[ ] Protect privileged scripts
[ ] Secure service binaries
[ ] Audit scheduled tasks
[ ] Remove unnecessary privileges
[ ] Protect credentials
[ ] Monitor privileged activity
```

---

## Key Takeaway

Privilege escalation usually involves a broken trust boundary:

```text
LOW-PRIVILEGED CONTROL
          +
PRIVILEGED EXECUTION
          ↓
POTENTIAL ESCALATION
```

Always determine exactly which permissions are available and whether a privileged process trusts lower-privileged resources.
