# LinPEAS

> Linux privilege-escalation enumeration notes for authorized labs and security assessments.

## What is LinPEAS?

**LinPEAS** is an automated Linux enumeration script from the PEASS-ng project.

Its purpose is to identify system information and potential privilege-escalation paths.

Conceptually:

```text
Manual Enumeration
        ↓
Many commands/checks
        ↓
LinPEAS
        ↓
Consolidated findings
```

LinPEAS primarily helps with **enumeration**.

A highlighted result is not automatically exploitable.

## What LinPEAS Checks

Typical areas include:

```text
System information
Users and groups
Sudo permissions
SUID/SGID binaries
Linux capabilities
Cron jobs
Services
Processes
Writable files/directories
Credentials/config files
Environment variables
Containers
Interesting software
Network information
Kernel information
```

## Manual Checks Before Automation

Always understand the underlying enumeration.

### Identity

```bash
whoami
id
groups
```

### System

```bash
uname -a
cat /etc/os-release
hostname
```

### Sudo

```bash
sudo -l
```

### SUID

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

### Cron

```bash
cat /etc/crontab
ls -la /etc/cron.d/
ls -la /etc/cron.daily/
```

### Processes

```bash
ps aux
```

### Services

```bash
systemctl --type=service --state=running
```

### Network

```bash
ip addr
ip route
ss -tulpn
```

## Typical Authorized-Lab Usage

Once LinPEAS is available on an authorized target:

```bash
chmod +x linpeas.sh
./linpeas.sh
```

Or:

```bash
bash linpeas.sh
```

Saving output can make later analysis easier:

```bash
./linpeas.sh | tee linpeas-output.txt
```

## Reading LinPEAS Output

Do not blindly chase every highlighted line.

Prioritize findings involving privilege boundaries.

### High-Value Areas

```text
sudo permissions
SUID/SGID
capabilities
writable privileged scripts
cron jobs
privileged services
credentials
container configuration
sensitive group membership
```

## Example Analysis Process

Suppose LinPEAS identifies:

```text
Interesting SUID binary
```

Do not immediately conclude:

```text
Privilege escalation possible
```

Instead:

```text
Identify binary
      ↓
Verify permissions
      ↓
Understand binary behavior
      ↓
Compare with known references
      ↓
Determine whether privilege boundary is reachable
```

## Sudo Findings

If LinPEAS highlights sudo configuration, verify manually:

```bash
sudo -l
```

Look for:

```text
NOPASSWD
unusual commands
wildcards
scripts
interpreters
editors
```

## Writable Files

Interesting writable files may include:

```text
service scripts
cron scripts
configuration files
startup files
application scripts
```

Verify:

```bash
ls -l <file>
ls -ld <directory>
namei -l <path>
```

## Credential Hunting

LinPEAS may identify potentially sensitive files or strings.

Common locations worth reviewing in an authorized assessment:

```text
configuration files
environment files
shell history
application directories
backup files
SSH configuration
```

Never assume a discovered string is a valid credential without validation appropriate to the assessment.

## Containers

LinPEAS can identify:

```text
Docker
LXC/LXD
container indicators
Docker socket
interesting group memberships
mounts
```

Useful manual checks:

```bash
id
groups
ls -l /var/run/docker.sock
mount
```

## False Positives

LinPEAS is intentionally aggressive about identifying interesting conditions.

Therefore:

```text
Highlighted != Vulnerable
```

Always verify findings manually.

## Recommended Workflow

```text
1. Perform basic manual enumeration
2. Run LinPEAS
3. Save output
4. Review high-confidence findings
5. Verify manually
6. Research unusual binaries/configurations
7. Prioritize realistic privilege boundaries
```

## Quick Manual Checklist

```bash
whoami
id
sudo -l
uname -a
cat /etc/os-release
ps aux
ss -tulpn
find / -perm -4000 -type f 2>/dev/null
getcap -r / 2>/dev/null
cat /etc/crontab
```

## Key Takeaway

LinPEAS is an **enumeration accelerator**.

Use:

```text
LinPEAS
   ↓
Potential findings
   ↓
Manual verification
   ↓
Technical analysis
   ↓
Confirmed finding
```

Do not replace understanding with automated output.
