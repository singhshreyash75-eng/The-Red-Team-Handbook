
# Metasploitable 2

> Intentionally vulnerable Linux virtual machine for network enumeration, service assessment, and penetration-testing practice.

## 1. What is Metasploitable 2?

**Metasploitable 2** is an intentionally vulnerable Linux virtual machine created for security training.

It contains numerous outdated or insecure services.

Common learning areas:

```text
Network Scanning
Service Enumeration
Vulnerability Identification
Authentication Security
Web Security
Privilege Escalation
```

Metasploitable 2 should never be exposed directly to the public internet.

---

## 2. Lab Architecture

Recommended:

```text
Attacker VM
    |
Host-Only Network
    |
Metasploitable 2 VM
```

Both systems should reside on an isolated lab network.

---

## 3. Download and Deployment

Obtain Metasploitable 2 from a trusted official training source.

It is commonly distributed as a VMware-compatible virtual machine image.

Possible virtualization platforms:

```text
VMware
VirtualBox
UTM / QEMU
```

Compatibility depends on host architecture and virtualization capabilities.

---

## 4. Apple Silicon Considerations

Metasploitable 2 is based on an older x86 Linux environment.

On Apple Silicon:

```text
ARM Host
   ↓
x86 Emulation
   ↓
Metasploitable 2
```

This may require QEMU emulation rather than native virtualization.

Performance can be significantly lower than native ARM guests.

---

## 5. Network Isolation

Use:

```text
Host-Only
```

or another dedicated isolated network.

Avoid exposing vulnerable services through bridged networking or public port forwarding.

Verify the VM's network configuration before starting any assessment.

---

## 6. Identify Target IP

Inside the lab VM:

```bash
ifconfig
```

or:

```bash
ip addr
```

Availability depends on the old operating-system environment.

Record the private lab IP address.

---

## 7. Initial Nmap Scan

Example against an assigned lab IP:

```bash
nmap -sV -sC -oN initial.txt 192.168.56.101
```

Collect:

```text
Open Ports
Services
Versions
Service Banners
NSE Results
```

---

## 8. Common Exposed Services

Metasploitable 2 commonly includes intentionally insecure or outdated services associated with:

```text
FTP
SSH
Telnet
HTTP
SMB
Database Services
RPC
NFS
Java Services
```

Actual exposure depends on the VM image and configuration.

---

## 9. FTP Enumeration

Identify service:

```bash
nmap -p 21 -sV 192.168.56.101
```

Review:

```text
FTP Version
Authentication Policy
Anonymous Access
Directory Permissions
```

Do not assume a service banner proves vulnerability.

---

## 10. SMB Enumeration

```bash
nmap -p 139,445 -sV 192.168.56.101
```

Inspect supported protocols:

```bash
nmap -p 445 --script smb-protocols 192.168.56.101
```

Review:

```text
Share Permissions
Guest Access
Legacy Protocols
Authentication
```

---

## 11. Web Enumeration

HTTP service discovery:

```bash
nmap -p 80,8080 -sV 192.168.56.101
```

Inspect web applications using:

```text
Browser
Burp Suite
Caido
curl
```

Look for:

```text
Application Versions
Authentication
Input Validation
Exposed Files
```

---

## 12. Database Services

Database services may include:

```text
MySQL
PostgreSQL
```

Review:

```text
Network Exposure
Authentication Configuration
User Privileges
Outdated Versions
```

Use only lab-approved credentials.

---

## 13. Vulnerability Research

Workflow:

```text
Service Banner
      ↓
Product and Version
      ↓
Vendor Advisory / CVE
      ↓
Affected Configuration?
      ↓
Controlled Validation
```

Useful resources:

```text
NVD
CVE Program
Exploit-DB
Vendor advisories
```

---

## 14. Metasploit Framework

Metasploitable 2 is commonly used alongside Metasploit for controlled vulnerability demonstrations.

Start:

```bash
msfconsole
```

Search for a product:

```text
search <product>
```

Review a module:

```text
info <module>
```

Before using a module, inspect its prerequisites and potential side effects.

---

## 15. Privilege Escalation

After obtaining authorized lab access, review:

```text
Current User
sudo Permissions
SUID Files
Services
Kernel Version
File Permissions
```

Example:

```bash
id
uname -a
```

SUID enumeration:

```bash
find /usr /bin /sbin -type f -perm -4000 2>/dev/null
```

---

## 16. Logging and Evidence

Record:

```text
IP Address
Open Ports
Service Versions
Potential Vulnerabilities
Validation Evidence
Security Impact
```

Avoid copying unnecessary credential material.

---

## 17. Snapshot Strategy

Before testing:

```text
Clean VM
   ↓
Create Snapshot
   ↓
Perform Lab Exercise
   ↓
Restore Snapshot
```

Snapshots allow repeatable exercises and reduce persistent lab changes.

---

## 18. Recommended Exercise

```text
1. Deploy isolated VM

2. Identify IP address

3. Perform host discovery

4. Enumerate open ports

5. Fingerprint services

6. Research outdated software

7. Validate approved findings

8. Review permissions

9. Document root causes

10. Restore snapshot
```

---

## 19. Lab Documentation

```text
Target:
Metasploitable 2

Network:
Isolated Lab

IP:
__________

Open Ports:
__________

Services:
__________

Potential CVEs:
__________

Validated Findings:
__________

Root Causes:
__________

Mitigations:
__________
```

---

## Key Takeaway

Metasploitable 2 teaches:

```text
NETWORK DISCOVERY
       ↓
SERVICE ENUMERATION
       ↓
VULNERABILITY RESEARCH
       ↓
CONTROLLED VALIDATION
```

Its intentionally insecure services make isolation essential.
