
# Active Directory — Cheat Sheet

> Quick reference for Active Directory enumeration, authentication, permissions, and security assessment in authorized environments.

## 1. Active Directory Fundamentals

**Active Directory (AD)** is Microsoft's directory service for managing identities, computers, and resources in Windows domains.

Important components:

- **Domain Controller (DC):** Server hosting AD DS and authentication services.
- **Domain:** Logical boundary for directory objects.
- **Forest:** Collection of one or more AD domains.
- **OU:** Organizational Unit for organizing directory objects.
- **GPO:** Group Policy Object.
- **LDAP:** Directory query protocol.
- **Kerberos:** Primary authentication protocol in modern AD domains.
- **DNS:** Critical for locating domain services.

```text
Forest
 └── Domain
      ├── Domain Controllers
      ├── Users
      ├── Groups
      ├── Computers
      └── Organizational Units
```

---

## 2. Important AD Ports

| Port | Protocol | Purpose |
|---|---|---|
| 53 | TCP/UDP | DNS |
| 88 | TCP/UDP | Kerberos |
| 135 | TCP | RPC Endpoint Mapper |
| 139 | TCP | NetBIOS |
| 389 | TCP/UDP | LDAP |
| 445 | TCP | SMB |
| 464 | TCP/UDP | Kerberos password changes |
| 636 | TCP | LDAPS |
| 3268 | TCP | Global Catalog |
| 3269 | TCP | Global Catalog over TLS |
| 5985 | TCP | WinRM HTTP |
| 5986 | TCP | WinRM HTTPS |

---

## 3. Basic Domain Enumeration

Current identity:

```cmd
whoami
whoami /all
```

Domain information:

```cmd
echo %USERDOMAIN%
echo %LOGONSERVER%
```

PowerShell:

```powershell
$env:USERDOMAIN
$env:LOGONSERVER
```

Domain Controller discovery:

```cmd
nltest /dclist:corp.example
```

Current domain information:

```powershell
Get-ADDomain
Get-ADForest
```

Requires the ActiveDirectory PowerShell module and appropriate access.

---

## 4. User Enumeration

List domain users:

```powershell
Get-ADUser -Filter *
```

Selected properties:

```powershell
Get-ADUser -Filter * |
Select-Object Name, SamAccountName, Enabled
```

Specific user:

```powershell
Get-ADUser -Identity alice -Properties *
```

Useful security-review fields:

```text
Enabled
PasswordLastSet
LastLogonDate
ServicePrincipalName
AdminCount
```

---

## 5. Group Enumeration

List groups:

```powershell
Get-ADGroup -Filter *
```

List group members:

```powershell
Get-ADGroupMember -Identity "Domain Admins"
```

Recursive membership:

```powershell
Get-ADGroupMember -Identity "Domain Admins" -Recursive
```

Review privileged groups such as:

```text
Domain Admins
Enterprise Admins
Schema Admins
Administrators
Account Operators
Backup Operators
```

---

## 6. Computer Enumeration

```powershell
Get-ADComputer -Filter *
```

Selected information:

```powershell
Get-ADComputer -Filter * -Properties OperatingSystem |
Select-Object Name, OperatingSystem
```

Domain Controller discovery:

```powershell
Get-ADDomainController -Filter *
```

---

## 7. Kerberos Fundamentals

Normal authentication:

```text
Client
  |
  | AS-REQ
  v
KDC
  |
  | AS-REP / TGT
  v
Client
  |
  | TGS-REQ
  v
KDC
  |
  | TGS-REP / Service Ticket
  v
Service
```

Important terms:

```text
KDC = Key Distribution Center
TGT = Ticket Granting Ticket
TGS = Ticket Granting Service
SPN = Service Principal Name
```

Inspect current Kerberos tickets:

```cmd
klist
```

Inspect ticket details:

```cmd
klist tickets
```

---

## 8. SPN Enumeration

Service Principal Names identify Kerberos services.

List SPNs using native Windows tooling:

```cmd
setspn -T corp.example -Q */*
```

PowerShell:

```powershell
Get-ADUser -LDAPFilter "(servicePrincipalName=*)" `
-Properties ServicePrincipalName |
Select-Object SamAccountName, ServicePrincipalName
```

Security considerations:

- Weak service-account passwords
- Excessive service-account privileges
- Legacy encryption configuration
- Unnecessary SPNs

---

## 9. Kerberoasting

**Kerberoasting** involves obtaining Kerberos service-ticket material and attempting offline password recovery.

Concept:

```text
SPN Account
    ↓
Service Ticket
    ↓
Offline Password Audit
```

Defensive controls:

- Strong random service-account passwords
- Group Managed Service Accounts (gMSAs)
- Least privilege
- Modern encryption
- Monitoring service-ticket activity

Relevant Windows security event:

```text
4769 — Kerberos Service Ticket Requested
```

---

## 10. AS-REP Roasting

Occurs when accounts do not require Kerberos preauthentication.

Relevant AD account flag:

```text
DONT_REQ_PREAUTH
```

Audit accounts:

```powershell
Get-ADUser -Filter {
    DoesNotRequirePreAuth -eq $true
}
```

Mitigation:

```text
Require Kerberos preauthentication
Use strong credentials
Monitor authentication anomalies
```

---

## 11. AD Permissions

Important permissions:

```text
GenericAll
GenericWrite
WriteDacl
WriteOwner
ResetPassword
AddMember
```

These permissions may create indirect privilege relationships.

Example:

```text
User A
  |
CanModify
  |
Group B
  |
Privileged Membership
```

Always review both direct and inherited permissions.

---

## 12. BloodHound and SharpHound

```text
SharpHound
    ↓
Collect AD Relationships
    ↓
BloodHound
    ↓
Graph Analysis
```

Important relationship types:

```text
MemberOf
AdminTo
HasSession
GenericAll
GenericWrite
CanRDP
CanPSRemote
```

Use graph findings to identify excessive privileges and administrative exposure.

---

## 13. Password and Account Policy

View domain password policy:

```powershell
Get-ADDefaultDomainPasswordPolicy
```

Review:

```text
MinimumPasswordLength
PasswordHistoryCount
LockoutThreshold
LockoutDuration
MaxPasswordAge
```

Fine-grained policies:

```powershell
Get-ADFineGrainedPasswordPolicy -Filter *
```

---

## 14. Group Policy

List GPOs:

```powershell
Get-GPO -All
```

Requires the GroupPolicy module.

View applied policies:

```cmd
gpresult /r
```

Generate HTML report:

```cmd
gpresult /h report.html
```

Review GPOs for:

- Excessive administrative rights
- Insecure scripts
- Weak security settings
- Unexpected policy delegation

---

## 15. Important AD Security Events

| Event ID | Meaning |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4672 | Special privileges assigned |
| 4720 | User account created |
| 4728 | Member added to global security group |
| 4732 | Member added to local security group |
| 4740 | Account locked out |
| 4768 | Kerberos TGT requested |
| 4769 | Kerberos service ticket requested |
| 4771 | Kerberos preauthentication failed |

---

## 16. AD Security Assessment Workflow

```text
Identify Domain
      ↓
Enumerate Users / Groups
      ↓
Enumerate Computers
      ↓
Review Privileged Accounts
      ↓
Inspect Kerberos Configuration
      ↓
Analyze ACLs
      ↓
Map Trust Relationships
      ↓
Identify Excessive Privileges
      ↓
Document Findings
```

---

## Key Takeaway

AD security revolves around:

```text
IDENTITY
   +
AUTHENTICATION
   +
GROUP MEMBERSHIP
   +
PERMISSIONS
   +
TRUST RELATIONSHIPS
```

The critical question:

**Which identities can directly or indirectly control privileged resources?**
