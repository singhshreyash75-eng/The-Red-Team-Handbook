# 📁 FTP Enumeration

> File Transfer Protocol (FTP) is a client-server protocol used to transfer files across networks. During enumeration, FTP can reveal accessible files, authentication weaknesses, and server configuration information.

---

## Overview

FTP commonly uses:

```text
21/TCP → Control connection
20/TCP → Traditionally associated with active-mode data transfer
```

FTP itself does not encrypt credentials or transferred data.

Modern environments generally prefer:

```text
SFTP
FTPS
```

where appropriate.

---

## FTP vs SFTP vs FTPS

| Protocol | Description |
|---|---|
| FTP | Traditional unencrypted file-transfer protocol |
| FTPS | FTP protected using TLS |
| SFTP | SSH File Transfer Protocol; runs over SSH |

Important:

**SFTP is not "FTP over SSH."**

It is a separate protocol provided through SSH.

---

## Initial Enumeration

Nmap:

```bash
nmap -sV -p 21 TARGET
```

Default scripts:

```bash
nmap -sC -sV -p 21 TARGET
```

Possible output:

```text
21/tcp open ftp vsftpd
```

---

## Banner Grabbing

A service may identify itself when connected to.

Example:

```bash
nc TARGET 21
```

Possible banner:

```text
220 FTP Server Ready
```

Depending on configuration, the banner may expose product/version information.

---

## FTP Client

Connect:

```bash
ftp TARGET
```

The server may request:

```text
Name:
Password:
```

---

## Anonymous Login

Some FTP servers intentionally support anonymous access.

Common username:

```text
anonymous
```

Conventionally, an email address may be requested as the password.

In labs, an empty/simple password is sometimes accepted depending on configuration.

Do not assume anonymous FTP is automatically vulnerable; public download servers may intentionally permit it.

---

## Nmap Anonymous Check

Nmap includes FTP-related scripts.

Example:

```bash
nmap --script ftp-anon -p 21 TARGET
```

If anonymous access is enabled, the script may identify accessible directories/files.

---

## Basic FTP Commands

After connecting:

```text
ls
```

List files.

```text
pwd
```

Current remote directory.

```text
cd directory
```

Change remote directory.

```text
get file.txt
```

Download an authorized file.

```text
put file.txt
```

Upload, if your authenticated account has permission and the assessment permits it.

```text
bye
```

Disconnect.

---

## Binary vs ASCII Mode

FTP historically supports different transfer modes.

For arbitrary binary files:

```text
binary
```

For text:

```text
ascii
```

Modern FTP clients usually handle common cases sensibly, but knowing the distinction can help troubleshoot corrupted transfers.

---

## Passive vs Active FTP

FTP uses separate control and data connections.

### Active Mode

Conceptually:

```text
Client → Server:21
Server → Client:Data Port
```

### Passive Mode

Conceptually:

```text
Client → Server:21
Client → Server:Server-selected data port
```

Passive mode is generally easier through NAT/firewalls.

---

## What to Look For

During authorized enumeration:

```text
Anonymous access
Readable files
Writable directories
Backup files
Configuration files
Server version
Banner information
Directory structure
```

---

## Interesting Files

Examples:

```text
backup.zip
config.ini
README.txt
users.csv
deployment.txt
```

Their security relevance depends on content and intended exposure.

Do not download unnecessary sensitive information.

---

## Writable FTP

If an authorized account can upload files, determine:

```text
Where are files stored?

Can they be accessed elsewhere?

Who consumes them?

Are executable files interpreted?
```

Writable FTP by itself does not automatically imply RCE.

Context determines impact.

---

## FTP and Web Servers

Occasionally, FTP may manage files served by a web application.

Conceptually:

```text
FTP Upload
    ↓
Web Root
    ↓
HTTP Access
```

If discovered in an authorized lab, carefully determine whether uploaded content is interpreted or simply served as static data.

Do not assume the directories are connected without evidence.

---

## Nmap FTP Scripts

List available scripts:

```bash
ls /usr/share/nmap/scripts/ftp*
```

Possible scripts may cover:

```text
Anonymous access
Banner information
Selected configuration checks
```

Read each script's documentation before execution.

---

## Version Research

Suppose:

```text
vsftpd X.Y
```

is identified.

Research:

```text
Vendor advisory
Distribution package
NVD
CISA KEV if applicable
```

Do not rely solely on a version banner.

Versions may be:

- Backported
- Modified
- Spoofed
- Patched by the distribution

---

## Enumeration Workflow

```text
21 Open
   ↓
Banner / Version
   ↓
Authentication
   ↓
Anonymous Access?
   ↓
Directory Listing
   ↓
Read Permissions
   ↓
Write Permissions
   ↓
Understand File Usage
```

---

## Notes Template

```text
Host:
10.10.10.30

Port:
21

Server:
vsftpd

Anonymous:
Allowed

Readable:
Public directory

Writable:
No

Interesting:
README references internal hostname
```

---

## Red Team Perspective

Beginner thinking:

```text
FTP
 ↓
Anonymous Login
 ↓
Vulnerable
```

Better thinking:

```text
FTP
 ↓
Who can authenticate?
 ↓
What can they access?
 ↓
What permissions exist?
 ↓
What information does the content reveal?
 ↓
How does FTP relate to other services?
```

Enumeration is about understanding relationships.

---

## Common Beginner Mistakes

- Assuming anonymous access is always unintended.
- Downloading everything.
- Ignoring write permissions.
- Confusing FTP, FTPS, and SFTP.
- Treating version banners as proof of vulnerability.
- Ignoring the relationship between FTP content and web applications.
- Testing credentials without authorization.

---

## Interview Questions

### What is FTP?

A protocol used for transferring files between client and server.

### Default FTP control port?

TCP 21.

### Is FTP encrypted?

Traditional FTP is not.

### FTP vs SFTP?

FTP is the traditional protocol; SFTP is a separate file-transfer protocol operating over SSH.

### What is anonymous FTP?

FTP access intentionally or accidentally available without a named authenticated user account.

### Active vs passive FTP?

They differ primarily in how the data connection is established.

---

## Quick Revision

```text
Port:
21/TCP

Check:

Banner
Version
Authentication
Anonymous
Files
Read permissions
Write permissions

Commands:

ftp TARGET
ls
pwd
cd
get
put

Nmap:

nmap -sC -sV -p 21 TARGET
```

---

## Practice

- TryHackMe Network Services
- Hack The Box Academy
- Metasploitable
- VulnHub
- Local FTP lab

---

## References

- RFC 959 — FTP
- Nmap Documentation
- Samba/Unix FTP client documentation
- Hack The Box Academy
- TryHackMe
