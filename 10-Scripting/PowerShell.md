# PowerShell

> PowerShell fundamentals for Windows administration, Active Directory, automation, and authorized security assessment workflows.

## What is PowerShell?

**PowerShell** is Microsoft's command shell and scripting language built around the .NET ecosystem.

It is widely used for:

```text
Windows administration
Active Directory
Automation
Cloud administration
System configuration
Security analysis
Incident response
```

Modern PowerShell is also cross-platform, although Windows remains especially important for security work.

---

# PowerShell vs CMD

CMD primarily works with textual command output.

PowerShell works heavily with:

```text
Objects
```

This is a fundamental difference.

Conceptually:

```text
CMD
 ↓
Text
```

versus:

```text
PowerShell
 ↓
.NET Objects
 ↓
Properties + Methods
```

---

# Cmdlets

PowerShell commands are commonly called:

```text
Cmdlets
```

Naming convention:

```text
Verb-Noun
```

Examples:

```powershell
Get-Process
Get-Service
Get-ChildItem
Get-Content
Set-Location
```

---

# Discover Commands

Search commands:

```powershell
Get-Command
```

Example:

```powershell
Get-Command *Process*
```

Help:

```powershell
Get-Help Get-Process
```

Detailed help:

```powershell
Get-Help Get-Process -Full
```

---

# Basic Navigation

Current directory:

```powershell
Get-Location
```

List files:

```powershell
Get-ChildItem
```

Change directory:

```powershell
Set-Location C:\Users
```

Common aliases include:

```text
pwd
ls
cd
```

But scripts are often clearer when using full cmdlet names.

---

# Variables

PowerShell variables begin with:

```text
$
```

Example:

```powershell
$name = "Alice"
```

Use:

```powershell
Write-Output $name
```

---

# Strings

Double quotes expand variables:

```powershell
$name = "Alice"

"Hello $name"
```

Output:

```text
Hello Alice
```

Single quotes are literal:

```powershell
'Hello $name'
```

Output:

```text
Hello $name
```

---

# Arrays

```powershell
$ports = @(80, 443, 8080)
```

Access:

```powershell
$ports[0]
```

Loop:

```powershell
foreach ($port in $ports) {
    Write-Output $port
}
```

---

# Hash Tables

Key/value data:

```powershell
$services = @{
    80  = "HTTP"
    443 = "HTTPS"
}
```

Useful for structured automation.

---

# Pipeline

PowerShell uses:

```text
|
```

but passes objects rather than merely plain text in many native PowerShell pipelines.

Example:

```powershell
Get-Process | Where-Object CPU -gt 100
```

Conceptually:

```text
Process Objects
      ↓
Filter by Property
      ↓
Matching Objects
```

---

# Properties

Example:

```powershell
Get-Process
```

returns process objects.

Inspect available properties:

```powershell
Get-Process | Get-Member
```

Possible properties include:

```text
Name
Id
CPU
Path
```

---

# Select-Object

Select properties:

```powershell
Get-Process |
Select-Object Name, Id
```

This is cleaner than parsing formatted text.

---

# Where-Object

Filter:

```powershell
Get-Service |
Where-Object Status -eq "Running"
```

Conceptually:

```text
All Services
     ↓
Condition
     ↓
Running Services
```

---

# ForEach-Object

Process pipeline objects:

```powershell
Get-Process |
ForEach-Object {
    $_.Name
}
```

`$_` represents the current pipeline object.

---

# Conditions

```powershell
if ($value -eq 10) {
    Write-Output "Equal"
}
else {
    Write-Output "Different"
}
```

Common comparison operators:

```text
-eq    equal
-ne    not equal
-gt    greater than
-lt    less than
-ge    greater/equal
-le    less/equal
-like  wildcard comparison
-match regex comparison
```

---

# For Loop

```powershell
for ($i = 0; $i -lt 5; $i++) {
    Write-Output $i
}
```

---

# Foreach Loop

```powershell
$hosts = @(
    "server01",
    "server02"
)

foreach ($hostName in $hosts) {
    Write-Output $hostName
}
```

---

# Functions

```powershell
function Get-Greeting {
    param(
        [string]$Name
    )

    Write-Output "Hello $Name"
}
```

Call:

```powershell
Get-Greeting -Name "Alice"
```

---

# Parameters

PowerShell supports strongly structured script/function parameters.

Example:

```powershell
param(
    [string]$Target
)
```

This makes scripts easier to reuse and validate.

---

# Files

List:

```powershell
Get-ChildItem
```

Read:

```powershell
Get-Content file.txt
```

Write:

```powershell
Set-Content file.txt "Hello"
```

Append:

```powershell
Add-Content file.txt "More"
```

---

# Recursive Search

```powershell
Get-ChildItem C:\Temp -Recurse
```

Filter by extension:

```powershell
Get-ChildItem C:\Temp -Recurse -Filter *.log
```

---

# Search Text

```powershell
Select-String -Path file.txt -Pattern "error"
```

Recursive example:

```powershell
Get-ChildItem -Recurse -File |
Select-String -Pattern "error"
```

Use sensitive-data searches only within authorized scope.

---

# Processes

```powershell
Get-Process
```

Specific:

```powershell
Get-Process powershell
```

Process information is useful for administration and security triage.

---

# Services

```powershell
Get-Service
```

Running:

```powershell
Get-Service |
Where-Object Status -eq "Running"
```

Service configuration is security-sensitive because many services execute with elevated privileges.

---

# System Information

Useful commands include:

```powershell
Get-ComputerInfo
```

Environment:

```powershell
Get-ChildItem Env:
```

Username:

```powershell
$env:USERNAME
```

Domain:

```powershell
$env:USERDOMAIN
```

---

# Network Information

Modern PowerShell provides networking cmdlets such as:

```powershell
Get-NetIPAddress
Get-NetRoute
Get-NetTCPConnection
```

These can help inspect local network state.

---

# Test-NetConnection

Connectivity test:

```powershell
Test-NetConnection example.com -Port 443
```

Conceptually:

```text
Host
 +
Port
 ↓
Connectivity Test
```

Useful for troubleshooting authorized services.

---

# Web Requests

PowerShell can make HTTP requests.

```powershell
Invoke-WebRequest https://example.com
```

REST APIs:

```powershell
Invoke-RestMethod https://api.example.com
```

These are valuable for:

```text
Automation
API clients
Cloud administration
Testing
```

---

# JSON

PowerShell handles JSON conveniently.

Convert object to JSON:

```powershell
$data | ConvertTo-Json
```

JSON to object:

```powershell
$json | ConvertFrom-Json
```

This is extremely useful for API automation.

---

# CSV

Import:

```powershell
Import-Csv users.csv
```

Export:

```powershell
$data | Export-Csv output.csv -NoTypeInformation
```

This makes PowerShell useful for administrative reporting.

---

# Error Handling

PowerShell supports:

```text
try
catch
finally
```

Example:

```powershell
try {
    Get-Content "file.txt" -ErrorAction Stop
}
catch {
    Write-Output "Unable to read file"
}
```

---

# Active Directory

PowerShell is especially important in AD environments.

If the Microsoft ActiveDirectory module is installed:

```powershell
Import-Module ActiveDirectory
```

Domain:

```powershell
Get-ADDomain
```

Forest:

```powershell
Get-ADForest
```

Users:

```powershell
Get-ADUser -Filter *
```

Groups:

```powershell
Get-ADGroup -Filter *
```

Computers:

```powershell
Get-ADComputer -Filter *
```

Use only with authorized directory access.

---

# Group Membership

Example:

```powershell
Get-ADGroupMember "Domain Admins"
```

For a principal:

```powershell
Get-ADPrincipalGroupMembership <username>
```

This is useful for administrative and security review.

---

# Execution Policy

PowerShell has:

```text
Execution Policy
```

Check:

```powershell
Get-ExecutionPolicy -List
```

Important:

```text
Execution Policy
!=
Strong security boundary
```

It is primarily a script-execution safety/administration feature, not a replacement for application control or endpoint security.

---

# PowerShell Language Mode

Check:

```powershell
$ExecutionContext.SessionState.LanguageMode
```

Possible environments may use restrictions such as:

```text
Constrained Language Mode
```

as part of broader security controls.

---

# Script Files

PowerShell scripts typically use:

```text
.ps1
```

Example:

```text
enumerate.ps1
```

Run according to the environment's approved execution configuration.

---

# Modules

PowerShell functionality can be packaged into:

```text
Modules
```

List:

```powershell
Get-Module -ListAvailable
```

Modules provide reusable:

```text
Functions
Cmdlets
Variables
Resources
```

---

# Remoting

PowerShell supports remote administration.

Conceptually:

```text
Administrator
     ↓
PowerShell Remoting
     ↓
Remote Windows Host
```

Remoting is commonly associated with:

```text
WinRM
```

It should be restricted to authorized administrative identities and networks.

---

# PowerShell Logging

Defensive environments can record substantial PowerShell telemetry.

Important capabilities include:

```text
Script Block Logging
Module Logging
Transcription
Process creation logs
AMSI-related inspection
```

This makes PowerShell highly visible in well-configured environments.

---

# AMSI

**AMSI = Antimalware Scan Interface**

AMSI enables supported applications, including PowerShell, to provide content to security products for inspection.

Conceptually:

```text
PowerShell Content
       ↓
AMSI
       ↓
Security Product
```

---

# PowerShell in Security

PowerShell is useful for both red and blue teams.

Red/security assessment:

```text
Enumeration
Automation
AD queries
API interaction
System inspection
```

Blue/administration:

```text
Incident response
Threat hunting
Configuration
Logging
Automation
```

---

# Object-Based Advantage

Instead of:

```text
Command
 ↓
Text
 ↓
grep-like parsing
```

PowerShell often provides:

```text
Cmdlet
 ↓
Object
 ↓
Property Selection
 ↓
Filtering
```

Example:

```powershell
Get-Process |
Where-Object CPU -gt 10 |
Select-Object Name, CPU
```

This is one of the most important PowerShell concepts.

---

# Security Considerations

When writing PowerShell:

```text
Validate input
Avoid unsafe command construction
Protect credentials
Avoid hardcoded secrets
Use least privilege
Handle errors
Use approved modules
Log appropriately
```

---

# Secrets

Avoid:

```powershell
$password = "MySecretPassword"
```

inside committed scripts.

Prefer appropriate:

```text
Secret stores
Credential managers
Managed identities
Secure automation mechanisms
```

depending on the environment.

---

# PowerShell vs Bash

```text
PowerShell
    |
Object-oriented pipeline
Windows / AD integration
.NET ecosystem
```

```text
Bash
    |
Text-oriented Unix pipelines
Linux automation
Command chaining
```

---

# PowerShell vs Python

```text
PowerShell
    |
Excellent Windows administration
AD / Microsoft ecosystem
```

```text
Python
    |
General-purpose programming
Cross-platform automation
Large security ecosystem
```

Both are valuable.

---

# Key Takeaway

PowerShell is fundamentally:

```text
CMDLETS
   +
OBJECTS
   +
PIPELINES
   +
.NET
   +
AUTOMATION
```

For security work, focus on:

```text
Get-Command
Get-Help
Get-Member
Where-Object
Select-Object
Processes
Services
Networking
Active Directory
HTTP / APIs
```

The most important mental shift from traditional shells is:

```text
PowerShell Pipeline
      ↓
Passes Structured Objects
      ↓
Not Just Text
```
