# 🌐 Subdomain Enumeration

> Subdomain enumeration is the process of discovering hostnames belonging to a domain. It is one of the highest-value reconnaissance techniques because organizations often expose applications outside their primary website.

---

## Overview

A company may own:

```text
example.com
```

but its real attack surface might include:

```text
www.example.com
api.example.com
admin.example.com
dev.example.com
staging.example.com
vpn.example.com
support.example.com
```

Each hostname may represent a completely different application or infrastructure component.

---

## Why Subdomains Matter

Subdomains commonly expose:

- APIs
- Admin Panels
- VPN Portals
- Development Systems
- Staging Applications
- Legacy Systems
- Authentication Services
- Documentation
- Cloud Applications

The main domain is only one part of the attack surface.

---

## Passive vs Active Enumeration

### Passive

Uses existing data sources.

Examples:

- Certificate Transparency
- Search engines
- Public datasets
- DNS databases

### Active

Directly queries authorized target infrastructure or attempts candidate names.

Examples:

- DNS resolution
- Controlled DNS wordlist discovery
- Virtual-host discovery

---

# Basic Workflow

```text
Root Domain
     ↓
Passive Discovery
     ↓
Additional Sources
     ↓
Merge
     ↓
Deduplicate
     ↓
DNS Resolution
     ↓
HTTP Probe
     ↓
Prioritize
```

---

# Subfinder

Subfinder is designed primarily for passive subdomain enumeration.

Basic:

```bash
subfinder -d example.com -silent
```

Save:

```bash
subfinder -d example.com -silent > subfinder.txt
```

---

# Certificate Transparency

TLS certificates can expose hostnames.

Example search:

```text
%.example.com
```

A commonly used interface is:

```text
crt.sh
```

Possible discoveries:

```text
mail.example.com
vpn.example.com
staging.example.com
```

Certificate records may be historical, so always validate discovered names.

---

# Amass

OWASP Amass supports extensive asset discovery and DNS enumeration workflows.

A typical passive enumeration pattern is:

```bash
amass enum -passive -d example.com
```

Tool options evolve, so consult the current Amass documentation for production usage.

---

# Assetfinder

Another lightweight source for related hostnames.

Typical usage:

```bash
assetfinder --subs-only example.com
```

Different tools often overlap heavily.

The objective is coverage, not collecting identical results repeatedly.

---

# Combine Results

Suppose you have:

```text
subfinder.txt
amass.txt
assetfinder.txt
```

Combine:

```bash
cat subfinder.txt amass.txt assetfinder.txt | sort -u > all-subs.txt
```

Now you have a deduplicated list.

---

# DNS Resolution

Passive datasets frequently contain stale records.

Resolve before deeper probing.

Simple manual check:

```bash
dig +short api.example.com
```

For larger authorized lists, use a DNS resolver/enumeration tool.

Conceptually:

```text
all-subs.txt
     ↓
DNS Resolution
     ↓
resolved-subs.txt
```

---

# HTTP Probing

Once authorized hostnames are validated, identify which ones expose HTTP(S).

Example:

```bash
httpx -l resolved-subs.txt -silent
```

Potential output:

```text
https://api.example.com
https://admin.example.com
https://dev.example.com
```

---

# Interesting Naming Patterns

Pay attention to words such as:

```text
admin
api
auth
beta
dev
internal
legacy
old
portal
preprod
stage
staging
test
uat
vpn
```

These do not guarantee vulnerability, but can help prioritize review.

---

# Subdomain Permutations

Organizations often use predictable naming conventions.

If you discover:

```text
api-dev.example.com
```

related environments might conceptually include:

```text
api-stage.example.com
api-test.example.com
api-prod.example.com
```

Permutation generation should be used carefully and only within authorized scope.

---

# DNS Wildcards

Some domains resolve arbitrary subdomains.

Example:

```text
random123.example.com
```

still returns an IP.

This is called wildcard DNS.

Why it matters:

A brute-force tool may report thousands of false positives.

Always test for wildcard behavior before trusting large result sets.

---

# CNAME Records

Check:

```bash
dig CNAME app.example.com
```

CNAMEs can reveal third-party services such as:

```text
Cloud providers
CDNs
SaaS platforms
Hosting platforms
```

They also help explain where an application is actually hosted.

---

# Subdomain Takeover Concept

A dangling DNS record can occur when:

```text
sub.example.com
      ↓
CNAME
      ↓
External Service
      ↓
Resource Deleted
```

The DNS record remains even though the referenced resource no longer exists.

Whether this becomes exploitable depends on the external provider and configuration.

Do not attempt to claim or modify third-party resources unless the engagement explicitly permits it.

---

# Recursive Enumeration

Sometimes a discovered subdomain itself has additional nested hostnames.

Example:

```text
example.com
   ↓
corp.example.com
   ↓
vpn.corp.example.com
```

Large organizations may use multiple levels of DNS hierarchy.

---

# Common Workflow

```bash
subfinder -d example.com -silent > subs.txt
```

Then:

```bash
sort -u subs.txt > subs-clean.txt
```

Then validate DNS and HTTP only for authorized assets.

The important workflow is:

```text
Discover
   ↓
Deduplicate
   ↓
Resolve
   ↓
Probe
   ↓
Analyze
```

---

# What to Record

For interesting assets:

```text
Hostname:
admin.example.com

IP:
192.0.2.10

HTTPS:
Yes

Title:
Admin Portal

Notes:
Authentication required
```

This is much more useful than a raw hostname list.

---

# Red Team Perspective

Subdomain enumeration is not about achieving the largest possible number.

Suppose Tool A finds:

```text
5,000 subdomains
```

but Tool B finds:

```text
40 valid applications
```

The second dataset may be far more useful.

Focus on:

```text
Valid
+
In Scope
+
Reachable
+
Interesting
```

---

# Common Beginner Mistakes

- Testing discovered assets without verifying scope.
- Trusting passive results without DNS validation.
- Ignoring wildcard DNS.
- Running only one passive source.
- Keeping duplicates.
- Ignoring nested subdomains.
- Treating every hostname as equally important.
- Confusing DNS discovery with virtual-host discovery.

---

# Interview Questions

### What is subdomain enumeration?

Discovering hostnames associated with a root domain.

### Passive vs active subdomain enumeration?

Passive relies on existing datasets; active techniques interact directly with authorized infrastructure.

### Why use Certificate Transparency?

Certificates frequently contain additional domain names.

### What is wildcard DNS?

A configuration where arbitrary subdomains resolve, potentially creating false positives.

### Why resolve passive results?

Passive sources often contain stale DNS records.

### What is a dangling DNS record?

A DNS record pointing to an external resource that no longer exists.

---

# Quick Revision

```text
Root Domain
     ↓
Subfinder / CT / Amass
     ↓
Merge
     ↓
sort -u
     ↓
Resolve DNS
     ↓
HTTP Probe
     ↓
Prioritize

Interesting:
admin
api
auth
dev
staging
test
vpn
```

---

# Practice

Practice against:

- TryHackMe recon labs
- Hack The Box Academy labs
- Domains you personally control
- Explicitly authorized bug bounty scopes

---

# References

- ProjectDiscovery Subfinder Documentation
- OWASP Amass Documentation
- Certificate Transparency Project
- OWASP Web Security Testing Guide
- Hack The Box Academy
- TryHackMe
