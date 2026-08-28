# 🔐 SSH Enumeration

> SSH (Secure Shell) is an encrypted protocol used for remote administration, command execution, tunneling, and secure file transfer. During enumeration, the objective is to understand the SSH implementation, authentication methods, configuration, and its relationship to the rest of the environment.

---

## Overview

SSH commonly runs on:

```text
22/TCP
```

However, administrators frequently move it to non-standard ports.

SSH replaced insecure remote-access protocols such as Telnet because SSH encrypts communication.

Common uses include:

- Remote administration
- Shell access
- File transfer
- Port forwarding
- Tunneling
- Git authentication
- Infrastructure automation

---

## Initial Enumeration

Start with service detection:

```bash
nmap -sV -p 22 TARGET
```

Default scripts:

```bash
nmap -sC -sV -p 22 TARGET
```

Possible output:

```text
22/tcp open ssh OpenSSH
```

Record:

```text
Product
Version
Operating-system clues
Authentication information
```

---

## Banner Grabbing

SSH usually exposes a protocol banner.

Example:

```bash
nc TARGET 22
```

Possible response:

```text
SSH-2.0-OpenSSH_X.Y
```

This may reveal:

- SSH implementation
- Version family
- Sometimes OS/package clues

Treat banners as clues rather than guaranteed facts.

---

## Nmap SSH Scripts

List available scripts:

```bash
ls /usr/share/nmap/scripts/ssh*
```

Useful script categories can include:

- Host-key enumeration
- Authentication-method discovery
- Algorithm enumeration

For example:

```bash
nmap --script ssh-hostkey -p 22 TARGET
```

Check your installed Nmap documentation for exact script availability.

---

## SSH Host Keys

SSH servers identify themselves using host keys.

Common algorithms include:

```text
RSA
ECDSA
Ed25519
```

Clients may display a fingerprint when connecting for the first time.

Example:

```text
SHA256:...
```

Host keys help clients detect unexpected server identity changes.

---

## Connecting

Authorized example:

```bash
ssh user@TARGET
```

Specific port:

```bash
ssh -p 2222 user@TARGET
```

---

## Verbose Mode

One of the most useful SSH troubleshooting/enumeration options is:

```bash
ssh -v user@TARGET
```

More verbosity:

```bash
ssh -vv user@TARGET
```

or:

```bash
ssh -vvv user@TARGET
```

Verbose output can reveal:

- Negotiated algorithms
- Authentication methods attempted
- Key locations
- Connection failures

---

## Authentication Methods

SSH may support:

```text
Password
Public Key
Keyboard Interactive
Certificates
```

The exact configuration depends on the server.

A hardened environment may disable password authentication entirely.

---

## Public-Key Authentication

Conceptually:

```text
Client
  │
Private Key
  │
  ▼
SSH Server
  │
Authorized Public Key
  │
  ▼
Authentication
```

Typical private-key filenames include:

```text
id_rsa
id_ed25519
```

Public counterparts commonly end in:

```text
.pub
```

---

## `authorized_keys`

On many Linux systems:

```text
~/.ssh/authorized_keys
```

contains public keys permitted to authenticate as that account.

Permissions on `.ssh` files matter because SSH may reject insecure configurations.

---

## SSH Configuration

System-wide OpenSSH server configuration commonly resides at:

```text
/etc/ssh/sshd_config
```

Settings may control:

```text
Port
PermitRootLogin
PasswordAuthentication
PubkeyAuthentication
AllowUsers
AllowGroups
```

Configuration should only be inspected when you legitimately have local access.

---

## SCP

SSH can securely transfer files.

Authorized example:

```bash
scp file.txt user@TARGET:/tmp/
```

Download:

```bash
scp user@TARGET:/tmp/file.txt .
```

---

## SFTP

SFTP is the SSH File Transfer Protocol.

Connect:

```bash
sftp user@TARGET
```

Remember:

```text
SFTP ≠ FTP
```

It is a separate protocol operating through SSH.

---

## SSH Keys During Assessments

During an authorized assessment, private keys may sometimes appear in:

- Backups
- Home directories
- Configuration archives
- Deployment systems

If you encounter a key, determine:

```text
Who owns it?
Is use authorized?
Is it encrypted?
What systems is it intended for?
```

Do not automatically try discovered credentials/keys against unrelated infrastructure.

---

## Encrypted Private Keys

Private keys can themselves be protected using passphrases.

Conceptually:

```text
Private Key
    +
Passphrase
```

provides additional protection if the key file is stolen.

---

## Known Hosts

SSH clients commonly maintain:

```text
~/.ssh/known_hosts
```

This records previously observed server host keys.

In an authorized post-compromise assessment, host history can sometimes provide infrastructure context.

---

## SSH Agent

SSH agents hold keys in memory so users don't repeatedly enter passphrases.

Check locally:

```bash
ssh-add -l
```

when assessing a system you are authorized to inspect.

Agent-related attack paths are more advanced and belong in later lateral-movement material.

---

## Port Forwarding

SSH supports tunneling.

Conceptually:

### Local Forward

```text
Your Machine
    ↓
SSH Server
    ↓
Internal Service
```

### Remote Forward

```text
Remote Side
    ↓
SSH Tunnel
    ↓
Your Side
```

### Dynamic Forward

SSH can also create a SOCKS proxy.

We'll cover pivoting and tunneling more deeply in the exploitation/post-exploitation material.

---

## Version Research

If enumeration reveals an SSH version:

```text
OpenSSH X.Y
```

do not immediately conclude it is vulnerable.

Check:

```text
Vendor advisory
Distribution package
Patch/backport information
Configuration
NVD
CISA KEV where relevant
```

Linux distributions frequently backport security fixes.

---

## Authentication Testing

Credential testing can easily cause:

- Account lockouts
- Monitoring alerts
- Service disruption

Only perform password spraying or brute-force testing when explicitly permitted by the engagement.

For ordinary enumeration, first determine:

```text
Does SSH exist?
Which implementation?
Which authentication methods?
What role does this host play?
```

---

## Enumeration Workflow

```text
22 Open
   ↓
Banner
   ↓
Implementation / Version
   ↓
Host Keys
   ↓
Authentication Methods
   ↓
Configuration Clues
   ↓
Relationship to Other Services
```

---

## Example Notes

```text
Host:
10.10.10.40

Port:
22

Service:
OpenSSH

Authentication:
Public key + password observed

Hostname:
WEB01

Notes:
Same host also exposes HTTP
```

---

## Red Team Perspective

SSH itself is often well hardened.

The more interesting question may be:

> Where do legitimate SSH credentials or keys come from?

Potential relationships:

```text
Web Application
      ↓
Configuration Leak
      ↓
Username / Key
      ↓
SSH
```

or:

```text
SMB Share
   ↓
Backup
   ↓
SSH Configuration
```

Enumeration becomes powerful when services are correlated.

---

## Common Beginner Mistakes

- Treating SSH as vulnerable just because port 22 is open.
- Immediately brute-forcing credentials.
- Trusting version banners as proof of a CVE.
- Ignoring public-key authentication.
- Confusing SFTP with FTP.
- Ignoring SSH's tunneling capabilities.
- Reusing discovered credentials outside authorized scope.

---

## Interview Questions

### What is SSH?

An encrypted protocol for remote administration and related secure communication.

### Default port?

TCP 22.

### SSH vs Telnet?

SSH encrypts communication; traditional Telnet does not.

### What is public-key authentication?

Authentication where possession of a private key proves identity against a configured public key.

### What is SFTP?

A file-transfer protocol provided through SSH.

### Why use `ssh -v`?

To inspect connection and authentication negotiation details.

---

## Quick Revision

```text
Port:
22/TCP

Check:
Banner
Version
Host keys
Authentication methods

Commands:

nmap -sC -sV -p 22 TARGET
nc TARGET 22
ssh -v user@TARGET
ssh -p PORT user@TARGET
sftp user@TARGET
scp FILE user@TARGET:/path

Remember:
Open SSH ≠ Vulnerable SSH
```

---

## Practice

- OverTheWire Bandit
- TryHackMe
- Hack The Box
- HTB Academy
- Your own Linux SSH server

---

## References

- OpenSSH Documentation
- RFC 4251 — SSH Protocol Architecture
- Nmap NSE Documentation
- Hack The Box Academy
