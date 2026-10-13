
# Hack The Box (HTB)

> Practical penetration-testing labs, Active Directory environments, vulnerability exploitation, and offensive-security skill development.

## 1. What is Hack The Box?

**Hack The Box (HTB)** is a cybersecurity training platform providing controlled environments for offensive and defensive security practice.

Website: https://www.hackthebox.com/

Common learning areas:

- Network enumeration
- Web application security
- Linux and Windows exploitation
- Privilege escalation
- Active Directory
- Cryptography
- Reverse engineering
- Digital forensics

Conceptually:

```text
Target Machine
      ↓
Reconnaissance
      ↓
Enumeration
      ↓
Vulnerability Analysis
      ↓
Initial Access
      ↓
Privilege Escalation
      ↓
Flag Capture
```

---

## 2. HTB Learning Environments

HTB provides different training formats.

### Machines

Realistic Linux or Windows targets designed to test multiple security skills.

### Challenges

Focused exercises covering topics such as:

```text
Web
Crypto
Pwn
Reversing
Forensics
Miscellaneous
```

### Academy

Structured courses and practical exercises organized into learning modules and paths.

### Pro Labs

Larger interconnected environments involving multiple systems, identities, and attack paths.

Availability depends on the platform's current offerings and subscription access.

---

## 3. Machine Difficulty

Common difficulty categories include:

```text
Easy
Medium
Hard
Insane
```

Difficulty is not solely determined by the number of vulnerabilities.

It may also depend on:

- Required enumeration depth
- Attack-chain complexity
- Obscure misconfigurations
- Exploit development
- Privilege relationships
- Technical knowledge

---

## 4. Lab Connectivity

HTB environments commonly use VPN-based or browser-hosted lab access.

Typical OpenVPN workflow:

```bash
sudo openvpn lab.ovpn
```

Verify interfaces:

```bash
ip addr
```

Check routing:

```bash
ip route
```

Use only the VPN configuration supplied for your authorized lab.

---

## 5. Initial Reconnaissance

Begin with target identification and port discovery.

```bash
nmap -sV -sC -oN initial.txt 10.10.10.10
```

Important information:

```text
Open ports
Services
Versions
Operating-system clues
Authentication mechanisms
```

Example:

```text
22/tcp   SSH
80/tcp   HTTP
445/tcp  SMB
```

---

## 6. Service Enumeration

Enumeration depends on discovered services.

| Service | Investigation |
|---|---|
| HTTP/HTTPS | Pages, APIs, technologies |
| SSH | Version and authentication configuration |
| SMB | Shares, permissions, protocol configuration |
| FTP | Authorized access and exposed files |
| DNS | Records and service discovery |
| LDAP | Directory structure and permissions |
| RDP | Exposure and authentication configuration |

The objective is to understand the environment before attempting exploitation.

---

## 7. Web Enumeration

Common tools:

```text
Burp Suite
Caido
FFUF
Gobuster
Feroxbuster
Katana
```

Example:

```bash
ffuf -u http://10.10.10.10/FUZZ -w paths.txt
```

Investigate:

```text
Authentication
Authorization
File uploads
Parameters
APIs
Application configuration
```

---

## 8. Vulnerability Research

After identifying software:

```text
Product
   ↓
Version
   ↓
CVE / Vendor Advisory
   ↓
Exploit Research
   ↓
Applicability Check
```

Useful references:

```text
NVD
Exploit-DB
Searchsploit
Vendor advisories
```

Example:

```bash
searchsploit "Product Name"
```

A public exploit is not automatic proof that the lab target is vulnerable.

---

## 9. Linux Privilege Escalation

Common areas:

```text
sudo permissions
SUID / SGID
Capabilities
Cron jobs
Writable scripts
Services
Credentials
Kernel vulnerabilities
```

Initial checks:

```bash
id
sudo -l
uname -a
```

SUID enumeration:

```bash
find /usr /bin /sbin -type f -perm -4000 2>/dev/null
```

---

## 10. Windows Privilege Escalation

Common areas:

```text
Local groups
User privileges
Service permissions
Scheduled tasks
File ACLs
Stored credentials
Patch status
```

Commands:

```cmd
whoami /all
systeminfo
```

PowerShell:

```powershell
Get-Service
Get-ScheduledTask
```

---

## 11. Active Directory Labs

AD environments introduce:

```text
Domain Controllers
Users
Groups
Computers
Kerberos
LDAP
SPNs
ACLs
Trusts
```

Typical assessment flow:

```text
Domain Discovery
      ↓
Identity Enumeration
      ↓
Privilege Mapping
      ↓
Authentication Analysis
      ↓
Attack-Path Validation
```

BloodHound can help visualize privilege relationships.

---

## 12. User and Root Flags

HTB machines commonly use proof files to demonstrate progress.

Conceptually:

```text
Initial Access
     ↓
User-Level Flag
     ↓
Privilege Escalation
     ↓
Administrator / Root Flag
```

Flag paths and formats depend on the challenge.

Do not publish active challenge flags.

---

## 13. Recommended Methodology

```text
1. Read scope and machine details

2. Establish lab connectivity

3. Perform initial reconnaissance

4. Enumerate each discovered service

5. Identify potential vulnerabilities

6. Research prerequisites

7. Validate within the lab

8. Obtain required proof

9. Document the complete attack path

10. Review mitigations
```

---

## 14. Documentation Template

```text
Machine:
__________

Difficulty:
__________

Operating System:
__________

IP Address:
__________

Open Ports:
__________

Services:
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

## 15. Common Mistakes

Avoid:

```text
Running exploits before enumeration
Ignoring unusual ports
Using incorrect wordlists
Assuming scanner results are confirmed
Skipping privilege analysis
Blindly copying walkthroughs
```

A good HTB methodology emphasizes understanding over tool execution.

---

## 16. Learning Strategy

Recommended progression:

```text
Easy Linux
    ↓
Easy Windows
    ↓
Web-focused Machines
    ↓
Privilege Escalation
    ↓
Medium Machines
    ↓
Active Directory
    ↓
Complex Multi-Host Labs
```

After solving a machine:

- Reconstruct the attack chain.
- Identify the root cause.
- Document key commands.
- Explain defensive mitigations.
- Reattempt without the walkthrough.

---

## Key Takeaway

Hack The Box develops the ability to combine multiple cybersecurity concepts into complete assessments.

```text
ENUMERATION
     +
RESEARCH
     +
VALIDATION
     +
PRIVILEGE ANALYSIS
     =
COMPLETE ATTACK PATH
```

The most valuable skill is understanding **why each step works**, not simply capturing the flag.
