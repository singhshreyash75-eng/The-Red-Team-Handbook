# GTFOBins

> Quick notes for using GTFOBins during authorized Linux security assessments and labs.

## What is GTFOBins?

**GTFOBins** is a reference catalog of Unix binaries whose legitimate functionality can have security implications when combined with unsafe permissions or privilege configurations.

It is especially useful when reviewing:

- `sudo` permissions
- SUID binaries
- Linux capabilities
- Restricted shells
- File read/write permissions

## Start With Enumeration

### Sudo

```bash
sudo -l
```

This shows commands the current user is permitted to execute through `sudo`.

Pay attention to:

```text
NOPASSWD
```

and unusual binaries/scripts.

### SUID

Find SUID files:

```bash
find / -perm -4000 -type f 2>/dev/null
```

### SGID

```bash
find / -perm -2000 -type f 2>/dev/null
```

### Capabilities

```bash
getcap -r / 2>/dev/null
```

## GTFOBins Workflow

```text
1. Enumerate sudo/SUID/capabilities
            ↓
2. Identify unusual privileged binaries
            ↓
3. Search the binary in GTFOBins
            ↓
4. Check which functionality/category applies
            ↓
5. Validate within the authorized lab
```

## Common GTFOBins Categories

Entries can contain functionality such as:

```text
Shell
Command
Sudo
SUID
Capabilities
File read
File write
File upload
File download
```

Not every technique works in every environment.

## Why Context Matters

Finding a binary listed by GTFOBins does **not automatically mean the system is vulnerable**.

Check:

- Is SUID actually set?
- Is the binary permitted through `sudo`?
- Is `NOPASSWD` configured?
- Which Linux capability is assigned?
- Is the relevant functionality available in that binary/version?
- Do filesystem permissions permit the required operation?

## High-Value Enumeration

```bash
id
sudo -l
find / -perm -4000 -type f 2>/dev/null
getcap -r / 2>/dev/null
```

Then investigate unusual findings.

## Example Analysis

Suppose:

```bash
sudo -l
```

returns an unusual binary that can run with elevated privileges.

Analysis process:

```text
sudo permission
      ↓
binary identification
      ↓
GTFOBins lookup
      ↓
applicable functionality
      ↓
privilege-boundary assessment
```

The important finding is not simply:

```text
"binary exists"
```

but:

```text
"binary has privileged execution conditions
and exposes functionality relevant to that privilege"
```

## Defensive Use

GTFOBins is also useful defensively.

Audit privileged configurations involving:

- Interpreters
- Editors
- Pagers
- Archive utilities
- File-management tools
- Scripting-capable programs

Mitigations include:

- Least-privilege `sudoers` rules
- Removing unnecessary SUID/SGID bits
- Removing unnecessary capabilities
- Using absolute command paths
- Protecting privileged scripts/configuration
- Regularly auditing privileged binaries

## Quick Checklist

```bash
id
sudo -l
find / -perm -4000 -type f 2>/dev/null
find / -perm -2000 -type f 2>/dev/null
getcap -r / 2>/dev/null
```

## Key Takeaway

GTFOBins is primarily a **reference**, not an automatic vulnerability scanner.

First determine the privilege boundary:

```text
Sudo / SUID / Capability
          ↓
Privileged binary
          ↓
Available functionality
          ↓
Security impact
```
