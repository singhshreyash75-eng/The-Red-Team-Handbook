
# TryHackMe

> Guided cybersecurity training, practical labs, penetration-testing fundamentals, and structured learning paths.

## 1. What is TryHackMe?

**TryHackMe (THM)** is an interactive cybersecurity learning platform providing guided lessons, labs, and practical exercises.

Website: https://tryhackme.com/

Common topics:

```text
Linux
Networking
Web Security
Reconnaissance
Enumeration
Privilege Escalation
Active Directory
Cryptography
Digital Forensics
SOC Operations
```

---

## 2. TryHackMe Learning Model

TryHackMe commonly organizes training into:

```text
Rooms
Modules
Learning Paths
Practical Labs
Challenges
```

Conceptually:

```text
Theory
   ↓
Demonstration
   ↓
Hands-On Task
   ↓
Answer / Flag
   ↓
Concept Reinforcement
```

---

## 3. Rooms

A **room** is a learning environment focused on one or more topics.

Examples of room themes:

```text
Linux Fundamentals
Network Services
Web Security
Nmap
Privilege Escalation
Active Directory
```

Some rooms are guided while others require more independent investigation.

---

## 4. Learning Paths

Learning paths combine multiple rooms into a structured curriculum.

Common focus areas include:

```text
Beginner Cybersecurity
Penetration Testing
Web Application Security
SOC / Blue Team
Security Engineering
```

Names and availability can change over time.

---

## 5. AttackBox

TryHackMe provides browser-accessible lab environments for eligible rooms/plans.

The AttackBox typically includes security tools and connectivity to relevant lab targets.

Conceptually:

```text
AttackBox
    |
Lab Network
    |
Target Machine
```

---

## 6. VPN Connectivity

Local machines may connect using an authorized TryHackMe VPN configuration.

Typical OpenVPN workflow:

```bash
sudo openvpn lab.ovpn
```

Check interface:

```bash
ip addr
```

Check routes:

```bash
ip route
```

Use the current platform instructions for your lab.

---

## 7. Initial Reconnaissance

Start with basic service enumeration.

Example:

```bash
nmap -sV -sC 10.10.10.10
```

Collect:

```text
Open ports
Services
Versions
HTTP applications
Authentication mechanisms
```

---

## 8. Linux Fundamentals

Important commands:

```bash
whoami
id
pwd
ls -la
cat /etc/os-release
ps aux
```

Permissions:

```bash
ls -l
```

Networking:

```bash
ip addr
ss -tuln
```

---

## 9. Networking Fundamentals

Important topics:

```text
TCP/IP
DNS
HTTP
HTTPS
SSH
SMB
FTP
Routing
Firewalls
```

Understand the purpose of each protocol before using enumeration tools.

---

## 10. Web Enumeration

Common tools:

```text
Burp Suite
Gobuster
FFUF
Feroxbuster
curl
```

Example:

```bash
gobuster dir -u http://10.10.10.10 -w paths.txt
```

Use only against the lab target.

---

## 11. Web Security Topics

Common vulnerabilities:

```text
SQL Injection
XSS
IDOR
Authentication Flaws
File Upload
Path Traversal
SSRF
Command Injection
```

The objective is to understand the root cause and validate it safely within the lab.

---

## 12. Linux Privilege Escalation

Common checks:

```bash
id
sudo -l
uname -a
```

SUID:

```bash
find /usr /bin /sbin -type f -perm -4000 2>/dev/null
```

Review:

```text
sudo
SUID
Capabilities
Cron
Services
Writable files
```

---

## 13. Windows Privilege Escalation

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

Important areas:

```text
Privileges
Groups
Service configuration
Scheduled tasks
ACLs
Patch status
```

---

## 14. Active Directory

Important concepts:

```text
Domain Controller
Users
Groups
Kerberos
LDAP
SPNs
GPOs
Trusts
ACLs
```

Example authorized domain enumeration:

```powershell
Get-ADDomain
```

Group members:

```powershell
Get-ADGroupMember "Domain Admins"
```

Requires appropriate module and permissions.

---

## 15. Nmap

Useful commands:

```bash
nmap 10.10.10.10
```

Service versions:

```bash
nmap -sV 10.10.10.10
```

Selected ports:

```bash
nmap -p 22,80,443 10.10.10.10
```

Host discovery:

```bash
nmap -sn 10.10.10.0/28
```

---

## 16. Burp Suite

Workflow:

```text
Browser
   ↓
Burp Proxy
   ↓
Capture Request
   ↓
Repeater
   ↓
Modify Input
   ↓
Observe Response
```

Burp is useful for understanding HTTP application behavior.

---

## 17. Cryptography

Common introductory topics:

```text
Encoding
Hashing
Caesar Cipher
ROT13
XOR
RSA
AES
```

Example Base64 decoding:

```bash
printf 'aGVsbG8=' | base64 -d
```

---

## 18. Digital Forensics

Common artifacts:

```text
PCAP
Logs
Images
Archives
Metadata
Disk Images
```

Useful tools:

```text
Wireshark
ExifTool
strings
file
```

---

## 19. Room-Solving Methodology

```text
Read Objective
      ↓
Understand Scope
      ↓
Review Theory
      ↓
Enumerate Lab
      ↓
Identify Relevant Concept
      ↓
Test Hypothesis
      ↓
Submit Answer
      ↓
Document Learning
```

---

## 20. Notes Template

```text
Room:
__________

Difficulty:
__________

Category:
__________

Objective:
__________

Tools:
__________

Important Commands:
__________

Key Findings:
__________

Root Cause:
__________

Lessons Learned:
__________
```

---

## 21. Learning Strategy

A practical progression:

```text
Linux / Networking
       ↓
Reconnaissance
       ↓
Web Fundamentals
       ↓
Privilege Escalation
       ↓
Windows Fundamentals
       ↓
Active Directory
       ↓
Intermediate Labs
```

Do not rush into advanced labs without understanding the underlying protocols.

---

## 22. TryHackMe vs Hack The Box

| TryHackMe | Hack The Box |
|---|---|
| Strong guided-learning focus | Strong independent-machine focus |
| Step-by-step rooms | More open-ended problem solving |
| Beginner-friendly material | Broad difficulty spectrum |
| Learning paths | Machines, challenges, Academy, labs |

Both platforms offer different formats, so this is a general comparison rather than an absolute distinction.

---

## 23. TryHackMe vs PortSwigger

```text
TryHackMe
    |
Broad cybersecurity learning
```

```text
PortSwigger
    |
Specialized web-application security
```

They complement each other.

---

## 24. Common Mistakes

Avoid:

```text
Copying answers without understanding
Skipping foundational concepts
Running random tools
Ignoring enumeration results
Failing to document findings
Overusing walkthroughs
```

---

## 25. Skill Development

After every room, answer:

```text
What did I learn?

Which tool was used?

Why did the technique work?

What assumptions were incorrect?

How would a defender prevent it?
```

This turns challenge completion into lasting knowledge.

---

## Key Takeaway

TryHackMe is particularly useful for structured hands-on learning.

```text
LEARN
   ↓
PRACTICE
   ↓
VERIFY
   ↓
DOCUMENT
   ↓
REPEAT
```

The goal is to progress from guided exercises toward independent cybersecurity problem-solving.
