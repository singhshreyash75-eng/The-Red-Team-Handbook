# 🌍 Domain Name System (DNS)

> DNS (Domain Name System) is the Internet's phonebook. Instead of remembering IP addresses, humans use domain names like `google.com`, and DNS translates them into IP addresses that computers understand.

---

# Overview

Every time you visit a website, your system performs a DNS lookup before making an HTTP or HTTPS request.

Example

```
google.com

↓

DNS Lookup

↓

142.250.xxx.xxx

↓

HTTP Request

↓

Website Opens
```

Without DNS, users would have to remember IP addresses for every website.

---

# Learning Objectives

After this chapter, you should understand:

- What DNS is
- How DNS works
- DNS Resolution Process
- Common DNS Record Types
- DNS Caching
- Recursive vs Authoritative DNS
- Zone Transfers
- DNS Enumeration
- Red Team Perspective

---

# Why DNS Matters

DNS is one of the first services attackers enumerate.

DNS can reveal:

- Subdomains
- Internal infrastructure
- Mail servers
- Third-party services
- Cloud resources
- Development environments

Most reconnaissance starts with DNS.

---

# DNS Resolution Process

When you visit:

```
https://example.com
```

The lookup process is:

```text
Browser

↓

Browser Cache

↓

Operating System Cache

↓

Recursive Resolver

↓

Root DNS Server

↓

TLD Server (.com)

↓

Authoritative DNS Server

↓

IP Address Returned

↓

Browser Connects
```

---

# DNS Components

## Recursive Resolver

Responsible for finding the correct IP address.

Examples

- Google DNS (8.8.8.8)
- Cloudflare DNS (1.1.1.1)

---

## Root Server

Knows where Top Level Domain servers are located.

Example

```
.com

.org

.net
```

---

## TLD Server

Responsible for domain extensions.

Examples

```
.com

.org

.edu

.gov
```

---

## Authoritative Server

Contains the actual DNS records for the domain.

Example

```
example.com
```

---

# Common DNS Record Types

| Record | Purpose |
|---------|----------|
| A | IPv4 Address |
| AAAA | IPv6 Address |
| MX | Mail Server |
| NS | Name Server |
| TXT | Verification / SPF / DKIM |
| CNAME | Alias |
| PTR | Reverse Lookup |
| SOA | Start of Authority |

---

## A Record

Maps a domain to an IPv4 address.

Example

```
example.com

↓

104.26.10.78
```

---

## AAAA Record

Maps a domain to an IPv6 address.

---

## MX Record

Mail Exchange Record.

Example

```
mail.google.com
```

---

## TXT Record

Contains text information.

Common Uses

- SPF
- DKIM
- Domain Verification

Attackers often inspect TXT records during reconnaissance.

---

## CNAME

Creates an alias.

Example

```
blog.example.com

↓

example.com
```

---

## NS Record

Shows the domain's Name Servers.

Example

```
ns1.example.com

ns2.example.com
```

---

## PTR Record

Reverse DNS Lookup.

IP

↓

Hostname

---

# DNS Cache

Operating systems cache DNS results.

Benefits

- Faster browsing
- Reduced DNS traffic

Flush DNS Cache

Linux

```bash
sudo systemd-resolve --flush-caches
```

Windows

```powershell
ipconfig /flushdns
```

---

# DNS Commands

## nslookup

```bash
nslookup google.com
```

---

## dig

```bash
dig google.com
```

Query specific record

```bash
dig MX google.com
```

---

## host

```bash
host google.com
```

---

## Reverse Lookup

```bash
dig -x 8.8.8.8
```

---

# Zone Transfer

A DNS Zone Transfer copies an entire DNS zone from one server to another.

If misconfigured, attackers can obtain:

- Subdomains
- Internal hosts
- Mail servers
- Infrastructure details

Example

```bash
dig axfr @ns1.example.com example.com
```

Successful zone transfers are rare today but are still worth testing.

---

# DNS Enumeration

Common tools:

- dig
- host
- nslookup
- dnsx
- Amass
- Subfinder
- Assetfinder
- dnsenum
- fierce

---

# Public DNS Servers

| Provider | Address |
|-----------|----------|
| Google | 8.8.8.8 |
| Google | 8.8.4.4 |
| Cloudflare | 1.1.1.1 |
| Cloudflare | 1.0.0.1 |
| Quad9 | 9.9.9.9 |

---

# Red Team Perspective

DNS is usually the first step during reconnaissance.

Common activities:

- Subdomain Enumeration
- Zone Transfer Testing
- Name Server Enumeration
- TXT Record Analysis
- Mail Server Discovery
- Reverse DNS Lookup
- Cloud Asset Discovery

Typical workflow:

```text
Target Domain

↓

Subfinder

↓

dnsx

↓

httpx

↓

Nuclei
```

---

# Useful Commands

Lookup A Record

```bash
dig A example.com
```

Lookup MX Record

```bash
dig MX example.com
```

Lookup TXT Record

```bash
dig TXT example.com
```

Lookup NS Records

```bash
dig NS example.com
```

Reverse Lookup

```bash
dig -x 8.8.8.8
```

---

# Common Beginner Mistakes

- Thinking DNS only stores IP addresses
- Ignoring TXT records
- Forgetting Reverse DNS
- Not checking NS records
- Skipping Zone Transfer testing

---

# Interview Questions

### What is DNS?

### Difference between Recursive and Authoritative DNS?

### What is an MX Record?

### What is a CNAME?

### What is a Zone Transfer?

### Difference between A and AAAA Records?

### Why do attackers enumerate DNS?

---

# Quick Revision

- DNS translates names into IP addresses.
- DNS resolution involves Root → TLD → Authoritative servers.
- A = IPv4
- AAAA = IPv6
- MX = Mail
- TXT = Text / SPF / DKIM
- CNAME = Alias
- Zone Transfers can reveal infrastructure.
- DNS enumeration is the first step in reconnaissance.

---

# Practice

TryHackMe

- DNS in Detail
- Intro to Networking

Hack The Box Academy

- DNS Enumeration

PortSwigger

- Host Header Labs

---

# References

- RFC 1034
- RFC 1035
- OWASP Testing Guide
- PortSwigger Academy
- Hack The Box Academy
- TryHackMe
