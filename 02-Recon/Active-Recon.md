# 🎯 Active Reconnaissance

> Active reconnaissance involves directly interacting with authorized target systems to verify assets, identify exposed services, and collect current technical information.

---

## Overview

Passive recon tells you what **might exist**.

Active recon determines what **currently responds**.

Example:

```text
Passive Source

↓

api.example.com

↓

DNS Resolution

↓

HTTPS Probe

↓

Live API
```

Because active recon generates traffic against the target, it must remain within the authorized scope and engagement rules.

---

## Passive vs Active Recon

| Passive | Active |
|---|---|
| Existing/public information | Direct interaction |
| Usually lower visibility | Target can log requests |
| May contain stale data | Provides current information |
| Broad discovery | Validation and deeper discovery |

---

## Active Recon Workflow

```text
Passive Assets
      ↓
DNS Resolution
      ↓
Reachability
      ↓
HTTP Probing
      ↓
Fingerprinting
      ↓
Content Discovery
      ↓
Crawling
      ↓
Prioritization
```

---

## DNS Resolution

First verify whether discovered hostnames resolve.

Basic:

```bash
dig api.example.com
```

Short output:

```bash
dig +short api.example.com
```

For a list, DNS tooling such as `dnsx` can help in authorized workflows.

Conceptually:

```text
100 discovered names
        ↓
DNS resolution
        ↓
63 valid names
```

Removing stale assets saves time.

---

## HTTP Probing

DNS resolution does not guarantee that a web service exists.

For authorized host lists, an HTTP probe can identify responding applications.

Example:

```bash
httpx -l hosts.txt -silent
```

Potential results:

```text
https://api.example.com
https://admin.example.com
http://legacy.example.com
```

Depending on the current tool version, additional flags may collect titles, status codes, or technologies.

Check official documentation for current syntax.

---

## Manual HTTP Inspection

Use:

```bash
curl -I https://example.com
```

Potential response:

```http
HTTP/2 200
server: nginx
content-type: text/html
set-cookie: session=...
```

Look at:

- Status codes
- Redirects
- Cookies
- Server headers
- Security headers
- Content types

---

## Technology Fingerprinting

Useful tools include:

```text
WhatWeb
Wappalyzer
httpx
```

Example:

```bash
whatweb https://example.com
```

Possible output:

```text
nginx
PHP
WordPress
Bootstrap
```

Fingerprinting is not always accurate.

Verify important conclusions manually.

---

## HTTP Status Codes During Recon

| Code | Recon Meaning |
|---:|---|
| 200 | Resource available |
| 301/302 | Redirect — follow it |
| 401 | Authentication required |
| 403 | Resource exists but access denied |
| 404 | Not found |
| 429 | Rate limiting |
| 500 | Server-side error |

A `403` can sometimes be more interesting than a `404` because it indicates that a resource may exist.

---

## Crawling

Crawlers follow application links and discover resources.

Example:

```bash
katana -u https://example.com
```

Potential output:

```text
/login
/account
/api/users
/assets/app.js
```

Crawling differs from brute-force content discovery because it follows known application references.

---

## Content Discovery

Applications may contain unlinked paths.

Example lab syntax:

```bash
ffuf -w wordlist.txt -u https://example.com/FUZZ
```

Potential discoveries:

```text
/admin
/backup
/api
/uploads
```

Use reasonable request rates and follow program/engagement rules.

---

## Virtual Hosts

Multiple applications can share one server.

Example:

```text
10.10.10.10

Host: www.example.com
Host: admin.example.com
Host: dev.example.com
```

The server may return different applications depending on the `Host` header.

This is covered further in:

```text
Virtual-Hosts.md
```

---

## Parameter Discovery

Look for input surfaces such as:

```text
?id=
?url=
?redirect=
?file=
?user=
```

Parameters become relevant later during vulnerability testing.

---

## JavaScript Collection

Modern web applications often expose important information through JavaScript.

Look for:

```text
.js
.bundle.js
app.js
main.js
```

Then analyze them for:

- Endpoints
- Routes
- Parameters
- Application features

---

## Rate Limiting

Active recon may trigger:

```text
HTTP 429 Too Many Requests
```

Do not immediately increase concurrency.

Respect:

- Scope rules
- Rate limits
- Engagement restrictions

Controlled recon is more useful than generating unnecessary noise.

---

## Keep Evidence

Example:

```text
Host:
admin.example.com

Status:
200

Title:
Administration Portal

Technology:
nginx

Authentication:
Login form

Interesting:
/api
/reset-password
```

This becomes useful during later enumeration.

---

## Red Team Perspective

Active recon should answer:

```text
What actually exists?

What responds?

What technology is running?

Which assets deserve deeper testing?
```

The goal isn't:

```text
Scan everything as fast as possible.
```

The goal is:

```text
Reduce the attack surface into high-value targets.
```

---

## Common Beginner Mistakes

- Ignoring scope.
- Using excessive request rates.
- Assuming DNS resolution means HTTP is running.
- Ignoring redirects.
- Ignoring `401` and `403` responses.
- Trusting fingerprints blindly.
- Running scanners without manually reviewing results.
- Mixing recon and exploitation too early.

---

## Interview Questions

### What is active reconnaissance?

Direct interaction with authorized target infrastructure to obtain current information.

### Passive vs active recon?

Passive uses existing information; active directly queries or probes the target.

### Why probe HTTP after DNS resolution?

A valid DNS record doesn't guarantee an active web service.

### Why is HTTP 403 interesting?

It can indicate that a resource exists even though access is denied.

---

## Quick Revision

```text
Resolve
   ↓
Probe
   ↓
Fingerprint
   ↓
Crawl
   ↓
Discover Content
   ↓
Analyze
   ↓
Prioritize
```

---

## Practice

Practice active reconnaissance only against:

- TryHackMe labs
- Hack The Box labs
- Local vulnerable applications
- Infrastructure you control
- Explicitly authorized programs

---

## References

- OWASP Web Security Testing Guide
- ProjectDiscovery Documentation
- Nmap Documentation
- Hack The Box Academy
- TryHackMe
