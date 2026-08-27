# 🔐 Cryptography Fundamentals

> Cryptography is the science of protecting information through mathematical techniques. As a Red Teamer, you don't need to become a cryptographer, but you must understand how encryption, hashing, and digital signatures work because almost every modern application relies on them.

---

# Overview

Cryptography provides secure communication over insecure networks.

It is used in:

- HTTPS
- VPNs
- SSH
- Password Storage
- JWT
- Digital Certificates
- Digital Signatures
- Blockchain
- Cloud Security

Without cryptography, secure online communication would not exist.

---

# Learning Objectives

After this chapter you should understand:

- Encryption
- Decryption
- Symmetric Encryption
- Asymmetric Encryption
- Hashing
- Salting
- Digital Signatures
- Certificates
- Public & Private Keys
- Red Team Perspective

---

# Core Concepts

Cryptography mainly consists of:

```
Encryption

↓

Hashing

↓

Digital Signatures

↓

Certificates
```

---

# Encryption

Encryption converts readable data into unreadable ciphertext.

```
Plain Text

↓

Encryption Algorithm

↓

Cipher Text

↓

Decryption

↓

Plain Text
```

---

# Why Encryption?

Imagine sending:

```
Password123
```

Without encryption:

Anyone intercepting the traffic can read it.

With encryption:

```
8xA29!aP...
```

Only the intended receiver can decrypt it.

---

# Symmetric Encryption

Uses **one key**.

```
Encrypt

↓

Shared Secret Key

↓

Decrypt
```

Advantages

- Fast
- Efficient

Disadvantages

- Key sharing problem

Examples

- AES
- DES (Deprecated)
- 3DES (Legacy)
- ChaCha20

---

# Asymmetric Encryption

Uses **two keys**.

```
Public Key

↓

Encrypt

↓

Private Key

↓

Decrypt
```

Advantages

- Secure key exchange
- Digital Signatures

Disadvantages

- Slower than symmetric encryption

Examples

- RSA
- ECC

---

# Symmetric vs Asymmetric

| Symmetric | Asymmetric |
|-----------|------------|
| One Key | Two Keys |
| Fast | Slower |
| AES | RSA |
| Encryption | Encryption + Authentication |

---

# Hashing

Hashing converts data into a fixed-length value.

```
Password123

↓

Hash Function

↓

482c811da5...
```

Important:

Hashing is **one-way**.

You cannot decrypt a hash.

---

# Common Hash Algorithms

| Algorithm | Status |
|-----------|---------|
| MD5 | Broken |
| SHA1 | Deprecated |
| SHA256 | Secure |
| SHA512 | Secure |
| bcrypt | Password Storage |
| scrypt | Password Storage |
| Argon2 | Recommended |

---

# Hash vs Encryption

| Hashing | Encryption |
|-----------|-------------|
| One-way | Two-way |
| Cannot decrypt | Can decrypt |
| Password Storage | Secure Communication |

---

# Salting

A salt is random data added before hashing.

Example

```
password

↓

Salt

↓

Hash
```

Purpose

Prevent:

- Rainbow Table attacks
- Duplicate hashes

---

# Digital Signatures

Digital Signatures prove:

- Authenticity
- Integrity

Example

```
File

↓

Hash

↓

Encrypt Hash

↓

Digital Signature
```

---

# Certificates

Certificates contain:

- Domain Name
- Public Key
- Issuer
- Expiry Date

Used by HTTPS.

---

# Public Key Infrastructure (PKI)

PKI consists of:

- Certificate Authority
- Certificates
- Public Keys
- Private Keys

Examples

- Let's Encrypt
- DigiCert

---

# Common Uses

HTTPS

↓

TLS

↓

SSH

↓

VPN

↓

JWT

↓

Password Storage

↓

Cloud

---

# Useful Commands

Generate SHA256

```bash
sha256sum file.txt
```

Generate MD5

```bash
md5sum file.txt
```

Generate RSA Key

```bash
openssl genrsa -out private.pem 2048
```

View Certificate

```bash
openssl s_client -connect google.com:443
```

Generate Self-Signed Certificate

```bash
openssl req -x509 -newkey rsa:2048 \
-keyout key.pem \
-out cert.pem
```

---

# Red Team Perspective

Understanding cryptography helps with:

- Password Cracking
- JWT Attacks
- TLS Analysis
- HTTPS Testing
- SSH Keys
- Certificate Analysis
- Hash Identification

Common Tools

- hashcat
- John the Ripper
- openssl
- CyberChef

---

# Common Beginner Mistakes

- Thinking hashing can be decrypted
- Using MD5 for passwords
- Ignoring salts
- Confusing encryption with encoding
- Confusing hashing with encryption

---

# Interview Questions

- Difference between hashing and encryption?
- What is salting?
- Why is MD5 insecure?
- Difference between symmetric and asymmetric encryption?
- What is RSA?
- What is AES?
- What is a Digital Signature?

---

# Quick Revision

- Encryption protects confidentiality.
- Hashing is one-way.
- AES = Symmetric Encryption.
- RSA = Asymmetric Encryption.
- bcrypt & Argon2 are preferred for password hashing.
- Certificates contain Public Keys.
- Salts protect against Rainbow Tables.

---

# Practice

TryHackMe

- Cryptography Basics

Hack The Box Academy

- Cryptography

PortSwigger

- JWT Labs

---

# References

- OWASP Cryptographic Storage Cheat Sheet
- NIST Cryptographic Standards
- OpenSSL Documentation
- RFC 8017 (RSA)
- RFC 8446 (TLS 1.3)
