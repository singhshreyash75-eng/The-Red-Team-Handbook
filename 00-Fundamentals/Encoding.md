# 🔤 Encoding Fundamentals

> Encoding is the process of converting data into another format so that it can be easily stored, transmitted, or processed by different systems. Unlike encryption, encoding is **not** designed to protect data.

---

# Overview

Encoding changes the representation of data.

It is used for:

- Data Transfer
- URLs
- Emails
- APIs
- Web Applications
- File Storage

Examples

- Base64
- URL Encoding
- Hexadecimal
- ASCII
- Unicode

---

# Learning Objectives

After reading this chapter you should understand:

- What Encoding is
- Difference between Encoding and Encryption
- Difference between Encoding and Hashing
- Base64
- URL Encoding
- Hex Encoding
- ASCII
- Unicode
- Common Red Team Uses

---

# Why Encoding Matters

Applications often encode data before sending it.

Example

```
Username

↓

Base64

↓

HTTP Request
```

Encoding makes data compatible with different systems.

It **does NOT** hide data.

---

# Encoding vs Encryption

| Encoding | Encryption |
|-----------|------------|
| Reversible | Reversible |
| No Secret Key | Uses Keys |
| Data Compatibility | Data Protection |
| Easy to Decode | Requires Decryption |

---

# Encoding vs Hashing

| Encoding | Hashing |
|-----------|---------|
| Reversible | One-way |
| No Security | Integrity |
| Can Decode | Cannot Decode |

---

# Base64 Encoding

One of the most common encodings used in web applications.

Example

```
Hello

↓

SGVsbG8=
```

Encode

```bash
echo "Hello" | base64
```

Decode

```bash
echo "SGVsbG8=" | base64 -d
```

---

# URL Encoding

URLs cannot contain certain special characters.

Example

```
Space

↓

%20
```

Example

```
https://example.com/?name=John%20Doe
```

Common Encodings

| Character | Encoded |
|-----------|----------|
| Space | %20 |
| / | %2F |
| ? | %3F |
| & | %26 |
| = | %3D |
| # | %23 |

---

# Hexadecimal Encoding

Represents data using Base-16.

Example

```
Hello

↓

48 65 6c 6c 6f
```

Convert using Python

```python
"Hello".encode().hex()
```

---

# ASCII

ASCII maps characters to numbers.

Example

| Character | Decimal |
|------------|---------:|
| A | 65 |
| B | 66 |
| C | 67 |

---

# Unicode

Unicode supports almost every language.

Examples

- English
- Hindi
- Chinese
- Japanese
- Arabic

UTF-8 is the most common encoding on the web.

---

# Binary

Everything eventually becomes binary.

Example

```
A

↓

01000001
```

---

# Common Encoding Examples

### Base64

```
admin

↓

YWRtaW4=
```

---

### URL

```
admin@test.com

↓

admin%40test.com
```

---

### Hex

```
admin

↓

61646d696e
```

---

# Useful Commands

Base64 Encode

```bash
echo "admin" | base64
```

Base64 Decode

```bash
echo "YWRtaW4=" | base64 -d
```

Python

```python
import base64

base64.b64encode(b"admin")
```

CyberChef

```
To Base64

From Base64

URL Encode

URL Decode
```

---

# Red Team Perspective

Encoding appears almost everywhere during penetration testing.

Examples

- JWT Tokens
- Cookies
- HTTP Parameters
- API Requests
- Basic Authentication
- Malware Analysis
- PowerShell Payloads

Example

```
Authorization:
Basic YWRtaW46cGFzc3dvcmQ=
```

Decoding reveals

```
admin:password
```

---

# Common Beginner Mistakes

- Thinking Base64 is encryption
- Thinking encoded data is secure
- Confusing hashing with encoding
- Forgetting URL decoding during testing

---

# Interview Questions

- What is Encoding?
- Difference between Encoding and Encryption?
- Difference between Encoding and Hashing?
- What is Base64?
- What is URL Encoding?
- Is Base64 secure?

---

# Quick Revision

- Encoding changes representation.
- Encoding is reversible.
- Base64 is NOT encryption.
- URL Encoding replaces unsafe URL characters.
- Hex represents Base-16 values.
- UTF-8 is the most common Unicode encoding.
- JWTs often contain Base64 encoded data.

---

# Practice

TryHackMe

- Intro to Web Hacking
- HTTP in Detail

PortSwigger

- JWT Labs

CyberChef

- https://gchq.github.io/CyberChef/

---

# References

- RFC 4648 (Base64)
- MDN URL Encoding
- CyberChef Documentation
