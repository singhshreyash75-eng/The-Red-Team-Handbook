# 🧭 Recon Methodology

> Recon should be a repeatable process, not a random collection of tools. The objective is to progressively transform a target scope into a prioritized map of assets worth investigating.

---

## Overview

Beginners often perform recon like this:

```text
Run Tool A
Run Tool B
Run Tool C
Collect thousands of URLs
```

That produces data, but not necessarily useful intelligence.

A better methodology is:

```text
Scope
  ↓
Discover
  ↓
Resolve
  ↓
Probe
  ↓
Fingerprint
  ↓
Crawl
  ↓
Analyze
  ↓
Prioritize
```

Every stage should reduce noise and improve your understanding of the target.

---

## 1. Understand the Scope

Before sending traffic, determine exactly what you are authorized to test.

Example:

```text
In Scope:
*.example.com

Out of Scope:
payments.example.com
third-party.example.net
```

Record:

- Root domains
- Allowed subdomains
- IP ranges
- Testing restrictions
- Rate limits
- Prohibited techniques

### Important Rule

**Discovery does not automatically mean authorization.**

A hostname discovered during recon may belong to a third party or fall outside the engagement scope.

Always verify scope before active testing.

---

## 2. Create a Workspace

Keep recon data organized.

Example:

```bash
mkdir -p recon/{domains,dns,http,urls,js,content,notes}
```

Possible structure:

```text
recon/
├── domains/
├── dns/
├── http/
├── urls/
├── js/
├── content/
└── notes/
```

This becomes particularly useful when thousands of assets are collected.

---

## 3. Start with Passive Recon

Begin by collecting information from public sources.

Useful sources include:

- Search engines
- Certificate Transparency logs
- WHOIS/RDAP
- Public DNS datasets
- GitHub
- Internet archives
- Public documentation

Goal:

```text
Root Domain
     ↓
Potential Assets
```

Example output:

```text
www.example.com
api.example.com
dev.example.com
support.example.com
```

Passive recon is covered in detail in `Passive-Recon.md`.

---

## 4. Subdomain Discovery

Subdomains significantly expand the visible attack surface.

Example:

```text
example.com

├── www.example.com
├── api.example.com
├── dev.example.com
├── admin.example.com
└── staging.example.com
```

A passive discovery example:

```bash
subfinder -d example.com -silent
```

Save results:

```bash
subfinder -d example.com -silent > subdomains.txt
```

Different sources discover different assets, so mature recon normally combines multiple authorized data sources and deduplicates the results.

---

## 5. Deduplicate Results

Recon tools frequently return duplicate entries.

Example:

```bash
sort -u subdomains.txt > subdomains-clean.txt
```

This reduces unnecessary requests later.

---

## 6. Resolve DNS

A discovered hostname does not necessarily still exist.

Resolve candidates before deeper testing.

Example:

```bash
dnsx -l subdomains-clean.txt -silent
```

Conceptually:

```text
Discovered Hostnames
        ↓
DNS Resolution
        ↓
Resolvable Hosts
```

This removes stale entries.

---

## 7. Probe HTTP Services

A resolvable hostname may or may not host a web application.

For authorized targets, HTTP probing can identify reachable web services.

Example:

```bash
httpx -l subdomains-clean.txt -silent
```

Useful information can include:

- HTTP status
- Page title
- Server headers
- Technologies
- Redirects

For example, current versions of tools may support additional metadata flags; check official documentation rather than relying on memorized syntax.

---

## 8. Read the Results — Don't Just Collect Them

Suppose you discover:

```text
https://www.example.com
https://admin.example.com
https://api.example.com
https://staging.example.com
```

Which deserves attention?

Potentially:

```text
admin
api
staging
```

because they may expose different functionality or security controls.

Recon is about **prioritization**, not merely volume.

---

## 9. Technology Fingerprinting

Determine what the application appears to use.

Examples:

```text
nginx
Apache
IIS
WordPress
React
Angular
Node.js
PHP
ASP.NET
```

Useful tools include:

- WhatWeb
- Wappalyzer
- httpx
- BuiltWith

Example:

```bash
whatweb https://example.com
```

Technology information helps guide later testing, but fingerprints should be treated as clues rather than absolute truth.

---

## 10. Inspect HTTP Manually

Automated tools are useful, but manually inspect interesting applications.

Check:

- Response headers
- Cookies
- Redirects
- Authentication
- Error messages
- Security headers
- Server information

Example:

```bash
curl -I https://example.com
```

Possible response:

```http
HTTP/2 200
server: nginx
content-type: text/html
strict-transport-security: max-age=31536000
```

---

## 11. Content Discovery

Applications often contain functionality that is not linked from the homepage.

Examples:

```text
/admin/
/api/
/backup/
/uploads/
/old/
/docs/
```

Tools such as `ffuf`, `feroxbuster`, and `gobuster` can assist with content discovery on authorized systems.

Example lab-style syntax:

```bash
ffuf -w wordlist.txt -u https://example.com/FUZZ
```

Detailed techniques belong in `Content-Discovery.md`.

---

## 12. Crawl the Application

Crawlers identify links, scripts, parameters, and endpoints already exposed by the application.

Example:

```bash
katana -u https://example.com
```

Potential discoveries:

```text
/login
/api/users
/assets/app.js
/upload
/account/reset
```

---

## 13. Historical URL Discovery

Current applications may no longer link to older endpoints that remain accessible.

Historical sources can reveal:

- Old APIs
- Legacy paths
- Previous parameters
- Deprecated functionality

Common tools:

```text
gau
waybackurls
```

Historical discovery should still be filtered against the current authorized scope before active testing.

---

## 14. JavaScript Recon

Modern applications often contain significant logic inside JavaScript bundles.

JavaScript can reveal:

- API endpoints
- Parameter names
- Feature flags
- Internal paths
- Third-party integrations
- Environment references

Typical workflow:

```text
Application
    ↓
Collect JS Files
    ↓
Inspect / Search
    ↓
Extract Interesting Endpoints
    ↓
Validate In-Scope Findings
```

This will be covered in `JavaScript-Recon.md`.

---

## 15. Parameter Discovery

Parameters create input surfaces.

Examples:

```text
?id=
?user=
?redirect=
?url=
?file=
?lang=
```

These can later become relevant when testing for vulnerabilities such as:

- IDOR
- Open Redirect
- SSRF
- File Inclusion
- SQL Injection

Parameter discovery will be covered separately.

---

## 16. Virtual Host Discovery

Sometimes multiple applications share the same IP address.

Example:

```text
10.10.10.10

↓

www.example.com
admin.example.com
dev.example.com
```

The HTTP `Host` header determines which application the server returns.

Virtual-host discovery can therefore reveal applications that aren't obvious from basic DNS enumeration.

---

## 17. Prioritize Assets

Not every asset deserves equal attention.

A simple priority model:

### High Priority

```text
admin
api
staging
dev
internal
vpn
auth
upload
```

### Medium Priority

```text
support
portal
docs
beta
```

### Lower Priority

Static marketing pages with minimal functionality.

Priority should always depend on actual functionality and scope, not merely the hostname.

---

## 18. Look for Interesting Differences

Compare applications.

For example:

```text
Production
https://example.com

Staging
https://staging.example.com
```

Potential differences:

- Authentication controls
- Debug messages
- Older versions
- Additional endpoints
- Test functionality

Development and staging systems can be particularly informative, but must still be explicitly in scope.

---

## 19. Keep Clean Recon Output

Example:

```text
Target: example.com

Resolved:
www.example.com
api.example.com
staging.example.com

Interesting:
api.example.com
staging.example.com

Technologies:
nginx
React
Node.js

Endpoints:
/api/v1/
/login
/upload

Next:
API authorization testing
Authentication testing
Content discovery
```

This is much more useful than storing 50 unorganized tool output files.

---

# Example Authorized Recon Pipeline

A basic pipeline might look like:

```bash
subfinder -d example.com -silent > subs.txt
```

Then:

```bash
sort -u subs.txt > subs-clean.txt
```

Then resolve/probe the **authorized** hosts using your preferred DNS and HTTP tools.

The important concept is:

```text
Discover
   ↓
Clean
   ↓
Resolve
   ↓
Probe
   ↓
Analyze
```

Do not blindly pipe tools together without understanding what each stage is doing.

---

# Recon Decision Tree

```text
Target
 │
 ├── Domains?
 │      └── Subdomain Discovery
 │
 ├── Web Applications?
 │      └── HTTP Probing
 │
 ├── Interesting App?
 │      ├── Crawl
 │      ├── Content Discovery
 │      ├── JS Analysis
 │      └── Parameter Discovery
 │
 ├── APIs?
 │      └── API Enumeration
 │
 └── Other Services?
        └── Enumeration Module
```

---

# What NOT to Do

Avoid the beginner pattern:

```text
Run 30 tools
      ↓
Generate 500 MB output
      ↓
Never inspect it
```

More tools do not automatically mean better recon.

Prefer:

```text
Small Dataset
     +
Understanding
     +
Prioritization
```

---

# Red Team Perspective

Good recon attempts to identify **relationships**.

For example:

```text
Employee GitHub
      ↓
Repository
      ↓
API Hostname
      ↓
Staging Application
      ↓
Authentication Portal
```

The value often comes from connecting multiple pieces of otherwise ordinary information.

---

# Common Beginner Mistakes

- Ignoring the scope.
- Treating every discovered hostname as authorized.
- Running too many overlapping tools.
- Keeping duplicate data.
- Ignoring JavaScript.
- Ignoring historical URLs.
- Focusing only on the main domain.
- Trusting technology fingerprints blindly.
- Collecting data without prioritizing it.
- Performing active scans before understanding engagement rules.

---

# Interview Questions

### What is reconnaissance?

Information gathering performed to understand a target and its attack surface.

### Passive vs Active Recon?

Passive recon relies primarily on existing/public information, while active recon directly interacts with target infrastructure.

### Why enumerate subdomains?

Different subdomains may expose independent applications, APIs, administrative interfaces, or development systems.

### Why resolve DNS before probing?

Historical or passive sources frequently contain stale hostnames.

### Why analyze JavaScript?

Client-side code often reveals endpoints, parameters, and application architecture.

### What is the biggest recon mistake?

Collecting large amounts of information without analyzing or prioritizing it.

---

# Quick Revision

```text
1. Confirm Scope
2. Passive Recon
3. Find Subdomains
4. Deduplicate
5. Resolve DNS
6. Probe Web Services
7. Fingerprint Technologies
8. Inspect Interesting Hosts
9. Crawl
10. Discover Content
11. Analyze JavaScript
12. Collect Parameters
13. Check Historical URLs
14. Prioritize
15. Move to Enumeration
```

---

# Practice

Good authorized environments for practicing reconnaissance concepts include:

- TryHackMe reconnaissance rooms
- Hack The Box Academy information-gathering modules
- Purpose-built local web labs
- Domains and infrastructure you personally control

---

# References

- OWASP Web Security Testing Guide
- NIST SP 800-115
- ProjectDiscovery Documentation
- OWASP Amass Documentation
- Hack The Box Academy
- TryHackMe
