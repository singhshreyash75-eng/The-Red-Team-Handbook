# LOLBAS

> Windows Living Off The Land Binaries, Scripts and Libraries notes for authorized security labs and assessments.

## What is LOLBAS?

**LOLBAS** stands for:

```text
Living Off The Land Binaries, Scripts and Libraries
```

It catalogs legitimate Windows components that expose functionality relevant to security testing.

Think of it roughly as:

```text
Linux   -> GTFOBins
Windows -> LOLBAS
```

The important point is that these binaries are normally **legitimate Microsoft/Windows components**.

## Why LOLBAS Matters

Built-in tools can sometimes perform operations beyond their most obvious purpose.

Relevant functionality can include:

```text
Execute
Download
Upload
File read
File write
Encode
Decode
Compile
Credentials
ADS
UAC-related behavior
```

The presence of a LOLBAS-listed binary alone does **not** prove a vulnerability.

Context and permissions matter.

## Basic Enumeration

Identify the current user:

```cmd
whoami
```

Privileges:

```cmd
whoami /priv
```

Groups:

```cmd
whoami /groups
```

System information:

```cmd
systeminfo
```

Environment:

```cmd
set
```

PowerShell equivalent:

```powershell
Get-ChildItem Env:
```

## Locate a Binary

CMD:

```cmd
where <binary>
```

PowerShell:

```powershell
Get-Command <binary>
```

Example:

```cmd
where certutil
```

## Common LOLBAS-Relevant Binaries

Examples frequently encountered during Windows analysis include:

```text
certutil.exe
bitsadmin.exe
mshta.exe
regsvr32.exe
rundll32.exe
msiexec.exe
wmic.exe
installutil.exe
msbuild.exe
powershell.exe
```

Availability varies by Windows version and configuration.

## Analysis Workflow

```text
Enumerate system
      ↓
Identify available binary
      ↓
Determine permissions/context
      ↓
Check LOLBAS reference
      ↓
Identify relevant functionality
      ↓
Validate only in authorized environment
```

## Important Questions

Before treating something as a finding, ask:

- Is the binary actually present?
- Which Windows version is running?
- What privileges does the current user have?
- Is the binary restricted by application control?
- Is PowerShell constrained?
- Is WDAC/AppLocker configured?
- Is the required functionality available in this version?
- Does the action cross a meaningful security boundary?

## Application Control

Useful areas to inspect include:

```text
AppLocker
Windows Defender Application Control (WDAC)
PowerShell execution policies
Constrained Language Mode
Microsoft Defender
EDR controls
```

PowerShell language mode:

```powershell
$ExecutionContext.SessionState.LanguageMode
```

Execution policy:

```powershell
Get-ExecutionPolicy -List
```

## Defensive Perspective

LOLBAS is useful for defenders because legitimate signed binaries can have powerful functionality.

Defensive controls include:

- Application allowlisting
- WDAC
- AppLocker
- Least privilege
- Command-line logging
- PowerShell logging
- Process creation auditing
- EDR monitoring
- Restricting unnecessary administrative privileges

## LOLBAS vs GTFOBins

```text
GTFOBins
    ↓
Unix/Linux binaries
    ↓
Sudo / SUID / Capabilities / file operations

LOLBAS
    ↓
Windows binaries/scripts/libraries
    ↓
Execution / transfer / encoding / other native functionality
```

## Key Takeaway

LOLBAS should be treated as a **reference database**, not as proof of compromise or vulnerability.

The useful workflow is:

```text
Binary
   +
Execution context
   +
Permissions
   +
Available functionality
   =
Security significance
```
