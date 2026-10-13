
# VulnHub

> Vulnerable virtual machines for offline penetration-testing practice, network enumeration, and privilege-escalation training.

## 1. What is VulnHub?

**VulnHub** is a platform that distributes intentionally vulnerable virtual-machine images for cybersecurity practice.

Website: https://www.vulnhub.com/

Typical learning areas:

```text
Linux Enumeration
Network Services
Web Security
Privilege Escalation
Vulnerability Research
Boot-to-Root Challenges
```

---

## 2. VulnHub vs Online CTF Platforms

VulnHub commonly emphasizes downloadable virtual machines.

Conceptually:

```text
Download VM
    ↓
Import Locally
    ↓
Configure Network
    ↓
Start Target
    ↓
Enumerate
    ↓
Solve
```

Unlike many cloud-hosted labs, the environment is often controlled locally.

---

## 3. VM Formats

VulnHub machines may be distributed in formats such as:

```text
OVA
OVF
VMDK
VHD
```

Compatibility depends on the virtualization platform.

Common options:

```text
VirtualBox
VMware
UTM / QEMU
```

---

## 4. Apple Silicon Considerations

Many older VulnHub images are x86/x86-64.

On Apple Silicon:

```text
ARM Host
    ↓
QEMU Emulation
    ↓
x86 Guest
```

Native ARM virtualization cannot directly run an x86 guest kernel.

Use compatible emulation or an x86 host.

---

## 5. Lab Networking

Recommended:

```text
Attacker VM
    |
Host-Only Network
    |
VulnHub VM
```

Avoid bridged networking unless the environment is carefully isolated and the rules permit it.

---

## 6. Snapshot

Before beginning:

```text
Clean VM
    ↓
Snapshot
    ↓
Assessment
    ↓
Restore
```

Snapshots allow repeatable experiments.

---

## 7. Discover Target IP

If the VM's address is unknown, inspect the lab's DHCP assignments or perform authorized host discovery on the isolated subnet.

Example:

```bash
nmap -sn 192.168.56.0/24
```

Confirm the target using VM/network configuration.

---

## 8. Initial Port Scan

```bash
nmap -sV -sC -oN initial.txt 192.168.56.101
```

Collect:

```text
Open Ports
Services
Versions
Operating-System Clues
```

---

## 9. Enumeration

Service-specific investigation:

```text
HTTP → Application Mapping
SMB → Shares and Permissions
FTP → Authentication and Files
SSH → Configuration
DNS → Records
```

Do not assume all services are vulnerable.

---

## 10. Web Enumeration

Useful tools:

```text
Burp Suite
Caido
FFUF
Gobuster
Feroxbuster
```

Example:

```bash
gobuster dir \
-u http://192.168.56.101 \
-w paths.txt
```

Only use against the isolated target.

---

## 11. Vulnerability Research

Workflow:

```text
Service
   ↓
Version
   ↓
CVE Research
   ↓
Applicability
   ↓
Controlled Validation
```

References:

```text
NVD
Vendor Advisories
Exploit-DB
Searchsploit
```

---

## 12. Linux Privilege Escalation

Common areas:

```text
sudo
SUID
Capabilities
Cron
Services
Writable Files
Kernel Vulnerabilities
```

Initial checks:

```bash
id
sudo -l
uname -a
```

---

## 13. Windows Privilege Escalation

For Windows-based practice machines, review:

```text
User Privileges
Group Membership
Service Permissions
Scheduled Tasks
ACLs
Patch Status
```

Commands:

```cmd
whoami /all
systeminfo
```

---

## 14. Flags

Some VulnHub machines contain proof files or flags.

Conceptually:

```text
Initial Access
     ↓
User Proof
     ↓
Privilege Escalation
     ↓
Root / Administrator Proof
```

The objective and format depend on the machine.

---

## 15. Writeup Methodology

A useful writeup should explain:

```text
Reconnaissance
Enumeration
Vulnerability
Initial Access
Privilege Escalation
Root Cause
Mitigation
```

Do not merely list commands.

Explain why each step was necessary.

---

## 16. Writeup Template

```text
Machine:
__________

Source:
VulnHub

Difficulty:
__________

Operating System:
__________

IP Address:
__________

Open Ports:
__________

Enumeration:
__________

Initial Access:
__________

Privilege Escalation:
__________

Root Cause:
__________

Mitigation:
__________

Lessons Learned:
__________
```

---

## 17. VulnHub vs Hack The Box

| VulnHub | Hack The Box |
|---|---|
| Downloadable local VMs | Commonly hosted lab environments |
| Local network configuration | VPN/browser-based access |
| Offline practice possible | Platform-managed exercises |
| User controls VM snapshots | Platform-specific reset mechanisms |

Both are valuable for practical penetration-testing development.

---

## 18. Common Mistakes

Avoid:

```text
Publicly exposing vulnerable VMs
Skipping snapshots
Using incompatible architectures
Ignoring service enumeration
Running random exploits
Failing to document findings
```

---

## 19. Recommended Workflow

```text
Choose VM
    ↓
Verify Image Source
    ↓
Import
    ↓
Configure Isolated Network
    ↓
Create Snapshot
    ↓
Discover IP
    ↓
Enumerate
    ↓
Research
    ↓
Validate
    ↓
Document
    ↓
Restore
```

---

## Key Takeaway

VulnHub develops practical skills in:

```text
NETWORK ENUMERATION
        +
VULNERABILITY ANALYSIS
        +
OPERATING-SYSTEM SECURITY
        +
PRIVILEGE ESCALATION
```

Its main advantage is the ability to operate a complete vulnerable machine inside a controlled local environment.
