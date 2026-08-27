# 🔌 Common Ports

> Ports identify the services running on a system. During reconnaissance and enumeration, identifying open ports is one of the first and most important steps.

---

# Overview

Every service running on a computer listens on a **port**.

Think of:

- IP Address → House Address
- Port → Door Number

Example

```
192.168.1.10:80
```

- IP = Device
- Port = HTTP Service

---

# Why Ports Matter

When performing a penetration test, the first questions are:

- Which ports are open?
- Which services are running?
- Which versions are installed?
- Are there any known vulnerabilities?

Open ports = Attack Surface.

---

# What is a Port?

A port is a **logical communication endpoint**.

Port Range

| Range | Purpose |
|---------|----------|
| 0–1023 | Well Known Ports |
| 1024–49151 | Registered Ports |
| 49152–65535 | Dynamic / Ephemeral Ports |

---

# TCP vs UDP Ports

Some services use TCP.

Some use UDP.

Example

```
HTTP → TCP

DNS → TCP & UDP

SSH → TCP

DHCP → UDP
```

---

# Most Common Ports

| Port | Protocol | Service | Red Team Notes |
|------:|----------|----------|----------------|
|20|TCP|FTP Data|Used with FTP|
|21|TCP|FTP|Anonymous login, file transfer|
|22|TCP|SSH|Remote shell, brute force|
|23|TCP|Telnet|Unencrypted remote login|
|25|TCP|SMTP|Mail server enumeration|
|53|TCP/UDP|DNS|Zone transfer, DNS enumeration|
|67|UDP|DHCP|IP assignment|
|68|UDP|DHCP Client|DHCP communication|
|69|UDP|TFTP|Unauthenticated file transfer|
|80|TCP|HTTP|Web applications|
|110|TCP|POP3|Email retrieval|
|111|TCP|RPC|NFS Enumeration|
|123|UDP|NTP|Time synchronization|
|135|TCP|MSRPC|Windows RPC|
|137|UDP|NetBIOS|Windows Enumeration|
|138|UDP|NetBIOS|File Sharing|
|139|TCP|NetBIOS|Legacy SMB|
|143|TCP|IMAP|Email|
|161|UDP|SNMP|Network Enumeration|
|389|TCP|LDAP|Active Directory|
|443|TCP|HTTPS|Secure Web Apps|
|445|TCP|SMB|Windows File Sharing|
|465|TCP|SMTPS|Secure SMTP|
|514|UDP|Syslog|Log Collection|
|587|TCP|SMTP Submission|Mail|
|636|TCP|LDAPS|Secure LDAP|
|993|TCP|IMAPS|Secure IMAP|
|995|TCP|POP3S|Secure POP3|
|1433|TCP|MSSQL|Database|
|1521|TCP|Oracle|Database|
|2049|TCP|NFS|Linux File Sharing|
|2375|TCP|Docker|Unauthenticated Docker API|
|3306|TCP|MySQL|Database|
|3389|TCP|RDP|Windows Remote Desktop|
|5432|TCP|PostgreSQL|Database|
|5900|TCP|VNC|Remote Desktop|
|5985|TCP|WinRM|Windows Remote Management|
|5986|TCP|WinRM SSL|Secure WinRM|
|6379|TCP|Redis|In-memory Database|
|8080|TCP|HTTP Alternate|Web Apps|
|8443|TCP|HTTPS Alternate|Secure Web Apps|
|9000|TCP|Common Web Service|Varies|

---

# Ports Frequently Seen During Pentests

### 21 - FTP

Check

- Anonymous Login
- Writable directories
- Sensitive files

Example

```bash
ftp TARGET
```

---

### 22 - SSH

Check

- Password brute force
- Weak credentials
- SSH version

Example

```bash
ssh user@IP
```

---

### 53 - DNS

Check

- Zone Transfer
- TXT Records
- Subdomains

---

### 80 / 443 - HTTP & HTTPS

Always inspect

- Login pages
- APIs
- Admin panels
- File uploads
- Hidden directories

---

### 139 / 445 - SMB

Check

- Shares
- Null Sessions
- SMB Version
- Anonymous Access

---

### 389 - LDAP

Common in Active Directory.

Check

- Anonymous bind
- Domain information

---

### 3306 - MySQL

Check

- Default credentials
- Version
- Remote access

---

### 3389 - RDP

Check

- Network Level Authentication
- Weak passwords
- Exposed service

---

# Useful Commands

Scan Common Ports

```bash
nmap IP
```

Scan All Ports

```bash
nmap -p- IP
```

Service Detection

```bash
nmap -sV IP
```

Aggressive Scan

```bash
nmap -A IP
```

Fast RustScan

```bash
rustscan -a IP
```

Netcat

```bash
nc -vz IP 22
```

List Listening Ports

```bash
ss -tuln
```

---

# Red Team Perspective

Open ports often determine your next steps.

Example

```
22

↓

SSH Enumeration

↓

Credentials

↓

Shell
```

```
80

↓

Web Enumeration

↓

Directory Fuzzing

↓

Burp Suite

↓

Vulnerability Discovery
```

```
445

↓

SMB Enumeration

↓

Shares

↓

Credentials

↓

Lateral Movement
```

Port enumeration defines the attack path.

---

# Common Beginner Mistakes

- Scanning only the top 1000 ports
- Ignoring UDP services
- Assuming every open port is vulnerable
- Forgetting version detection
- Not checking alternate HTTP ports

---

# Interview Questions

- What is a port?
- Difference between TCP and UDP ports?
- Why is Port 445 important?
- What runs on Port 389?
- Difference between HTTP and HTTPS ports?
- Why scan all 65535 ports?

---

# Quick Revision

- Ports identify services.
- Port 22 → SSH
- Port 53 → DNS
- Port 80 → HTTP
- Port 443 → HTTPS
- Port 445 → SMB
- Port 389 → LDAP
- Use `nmap -sV` for service detection.
- Always enumerate before exploiting.

---

# Practice

TryHackMe

- Nmap
- Network Services

Hack The Box Academy

- Service Enumeration

PortSwigger

- Web Security Academy

---

# References

- Nmap Documentation
- IANA Port Registry
- Hack The Box Academy
- TryHackMe
