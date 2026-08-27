# 🔒 HTTPS (HyperText Transfer Protocol Secure)

> HTTPS is the secure version of HTTP. It encrypts communication between clients and servers using TLS (Transport Layer Security), protecting data from interception and tampering.

---

# Overview

Whenever you visit a website using:

```
https://example.com
```

the browser establishes an encrypted connection before any sensitive data is exchanged.

Examples:

- Internet Banking
- Gmail
- GitHub
- Facebook
- APIs
- Cloud Dashboards

Without HTTPS, usernames, passwords and cookies can be intercepted.

---

# Learning Objectives

After this chapter you should understand:

- Difference between HTTP & HTTPS
- TLS
- SSL vs TLS
- Certificates
- Certificate Authorities
- TLS Handshake
- Encryption
- Common HTTPS Misconfigurations
- Red Team Perspective

---

# HTTP vs HTTPS

| HTTP | HTTPS |
|------|-------|
| Port 80 | Port 443 |
| Unencrypted | Encrypted |
| Easy to sniff | Protected using TLS |
| No identity verification | Uses certificates |

---

# Why HTTPS Matters

Without HTTPS:

```text
Browser
      │
Plain Text
      │
Attacker
      │
Server
```

The attacker can read everything.

With HTTPS:

```text
Browser
      │
Encrypted
      │
Attacker ❌
      │
Server
```

The attacker only sees encrypted data.

---

# SSL vs TLS

SSL (Secure Sockets Layer)

↓

Deprecated

↓

TLS (Transport Layer Security)

Today, almost every secure website uses TLS.

---

# TLS

TLS provides:

- Confidentiality
- Integrity
- Authentication

Meaning:

- Nobody can read the traffic.
- Nobody can modify the traffic.
- The client verifies it is talking to the correct server.

---

# Certificates

Every HTTPS website has a certificate.

Example

```
example.com
```

contains:

- Domain Name
- Public Key
- Expiry Date
- Issuer
- Digital Signature

View certificate:

Click the 🔒 icon in your browser.

---

# Certificate Authority (CA)

A Certificate Authority verifies website identities.

Popular CAs:

- Let's Encrypt
- DigiCert
- GlobalSign
- Sectigo

The browser trusts certificates signed by trusted CAs.

---

# Public Key Cryptography

HTTPS uses two keys:

Public Key

↓

Encrypt

Private Key

↓

Decrypt

The public key can be shared with everyone.

The private key must remain secret.

---

# TLS Handshake

Before encrypted communication begins:

```text
Browser

↓

Client Hello

↓

Server Hello

↓

Certificate

↓

Key Exchange

↓

Secure Session

↓

Encrypted HTTP
```

Once complete, normal HTTP traffic flows inside the encrypted tunnel.

---

# Cipher Suites

A cipher suite defines:

- Encryption Algorithm
- Key Exchange
- Authentication
- Hashing Algorithm

Modern websites use strong cipher suites to prevent attacks.

---

# HSTS

HTTP Strict Transport Security

Header:

```http
Strict-Transport-Security
```

Purpose:

Force browsers to always use HTTPS.

Prevents downgrade attacks.

---

# Common HTTPS Headers

```http
Strict-Transport-Security

Content-Security-Policy

X-Frame-Options

X-Content-Type-Options

Referrer-Policy
```

These improve overall security.

---

# HTTPS Misconfigurations

Common issues:

- Expired Certificates
- Self-Signed Certificates
- Weak TLS Versions
- Weak Cipher Suites
- Missing HSTS
- Mixed Content (HTTP resources on HTTPS pages)

---

# Mixed Content

Example:

Website:

```
https://example.com
```

Loads:

```
http://example.com/logo.png
```

This weakens security and may expose users to attacks.

---

# Red Team Perspective

During web assessments, check for:

- Expired certificates
- Weak TLS versions
- Missing HSTS
- Weak cipher suites
- Self-signed certificates
- Certificate information leakage

Tools:

```bash
sslscan target.com

nmap --script ssl-enum-ciphers target.com

openssl s_client -connect target.com:443
```

---

# Useful Commands

View certificate

```bash
openssl s_client -connect google.com:443
```

Check supported ciphers

```bash
nmap --script ssl-enum-ciphers target.com
```

SSL Scan

```bash
sslscan target.com
```

---

# Common Beginner Mistakes

- Thinking HTTPS makes websites "hack-proof"
- Ignoring certificate warnings
- Confusing SSL with TLS
- Believing HTTPS prevents all attacks
- Ignoring security headers

---

# Interview Questions

- Difference between HTTP and HTTPS?
- What is TLS?
- SSL vs TLS?
- What is a Certificate Authority?
- What is HSTS?
- Explain the TLS Handshake.
- What is Mixed Content?

---

# Quick Revision

- HTTPS uses TLS.
- Default Port: 443.
- Certificates verify server identity.
- Public Key encrypts.
- Private Key decrypts.
- HSTS forces HTTPS.
- SSL is deprecated.
- HTTPS provides confidentiality, integrity and authentication.

---

# Practice

- PortSwigger Web Security Academy
- TryHackMe — HTTP in Detail
- Hack The Box Academy — Web Requests

---

# References

- RFC 8446 (TLS 1.3)
- Mozilla SSL Configuration Guide
- OWASP TLS Cheat Sheet
- PortSwigger Academy
