# Feroxbuster

> Fast recursive web-content discovery notes for authorized security testing.

## What is Feroxbuster?

**Feroxbuster** is a web-content discovery tool designed for fast, recursive enumeration.

It searches web servers for hidden or unlinked:

```text
Directories
Files
Endpoints
Resources
```

Conceptually:

```text
Target
  |
Wordlist
  |
  v
Content Discovery
  |
  v
Interesting Paths
```

---

# Core Strength

Feroxbuster's major strength is:

```text
Recursive Discovery
```

Suppose it discovers:

```text
/api/
```

It can continue looking underneath it:

```text
/api/users
/api/admin
/api/v1
/api/docs
```

Conceptually:

```text
/
|
+-- api/
|    |
|    +-- v1/
|    |    |
|    |    +-- users
|    |
|    +-- docs/
|
+-- admin/
```

---

# Basic Usage

A common pattern:

```bash
feroxbuster -u https://example.test
```

Use only against authorized targets.

Check options:

```bash
feroxbuster -h
```

because capabilities and flags can evolve between versions.

---

# Custom Wordlist

Conceptually:

```bash
feroxbuster -u https://example.test -w wordlist.txt
```

Wordlist selection strongly affects:

```text
Coverage
Traffic
Runtime
False positives
```

---

# Wordlists

Useful categories include:

```text
General web content
API endpoints
Technology-specific paths
Backup filenames
Administrative paths
```

Large lists are not automatically better.

Use:

```text
Application fingerprint
      ↓
Appropriate Wordlist
```

---

# Recursive Enumeration

Traditional one-level discovery:

```text
/
 |
 +-- admin
 +-- api
```

Recursive discovery:

```text
/
 |
 +-- admin/
 |      |
 |      +-- users/
 |
 +-- api/
        |
        +-- v1/
             |
             +-- status
```

This can discover deeper content automatically.

---

# Why Recursion Matters

Web applications often expose functionality beneath discovered directories.

Example:

```text
/api
```

alone tells little.

But:

```text
/api/v1/users
/api/v1/orders
/api/docs
```

reveals much more about the application.

---

# Status Codes

Important responses:

```text
200 OK
204 No Content

301 Redirect
302 Redirect

401 Unauthorized
403 Forbidden

404 Not Found
```

Do not automatically ignore:

```text
401
403
```

because they may identify valid protected content.

---

# 403 Does Not Mean Vulnerability

A response:

```text
403 Forbidden
```

usually means:

```text
Resource may exist
but current request is denied
```

That is not itself a security flaw.

Record it as an interesting endpoint and analyze authorization only within scope.

---

# Redirects

Directories often redirect:

```text
/admin
```

to:

```text
/admin/
```

Therefore:

```text
301 / 302
```

responses can be useful during discovery.

---

# Extensions

Applications may contain files with extensions such as:

```text
.php
.html
.txt
.json
.xml
.js
```

Technology fingerprinting should guide which extensions are worth checking.

---

# Backup Files

Developers sometimes leave accidental artifacts such as:

```text
backup
old
copy
archive
```

Security review may look for exposed backup/configuration files using appropriate wordlists.

However, do not download or access sensitive data beyond what the assessment authorizes.

---

# Filters

Web servers can generate noisy responses.

Feroxbuster supports filtering behavior.

Useful response properties include:

```text
Status
Size
Words
Lines
```

Check:

```bash
feroxbuster -h
```

for exact options in the installed version.

---

# Custom 404 Problem

Some servers return:

```text
200 OK
```

for nonexistent paths.

Example:

```text
/not-real-123
     ↓
200 OK
"Page not found"
```

This creates false positives.

Therefore first understand the baseline response.

---

# Baseline

Before large enumeration:

```text
Random nonexistent path
        ↓
Observe response
        ↓
Status
Size
Content
Redirect
```

Then configure filters accordingly.

---

# Wildcard Routing

Modern frameworks may route every unknown URL to the same application.

Example:

```text
/random1 -> 200
/random2 -> 200
/random3 -> 200
```

Feroxbuster may then generate many false positives.

Filtering and manual validation become essential.

---

# Concurrency

Feroxbuster is designed to be fast.

But:

```text
More concurrency
      ↓
More requests/sec
      ↓
More server load
```

Do not maximize speed automatically.

Respect:

```text
Rate limits
Rules of engagement
Application capacity
```

---

# Depth

Recursive enumeration can grow rapidly.

Conceptually:

```text
Depth 0
/

Depth 1
/api

Depth 2
/api/v1

Depth 3
/api/v1/users
```

Restrict depth when broad recursion creates unnecessary traffic.

---

# Authentication

Applications may expose different content after login.

Authenticated content discovery conceptually requires:

```text
Authorized Session
       ↓
Feroxbuster
       ↓
Protected Application Paths
```

Treat cookies/tokens as credentials.

Avoid leaking them into:

```text
Shell history
Shared logs
Screenshots
Repositories
```

---

# Headers

Some applications require:

```text
Authorization
Cookies
Custom API headers
```

Feroxbuster can be configured to send appropriate headers where supported.

Always check current help/documentation for exact syntax.

---

# Proxy

During debugging, requests may be routed through:

```text
Burp Suite
Caido
```

Conceptually:

```text
Feroxbuster
     ↓
Intercepting Proxy
     ↓
Target
```

This helps understand how requests are being constructed.

---

# Feroxbuster + Burp

A strong workflow:

```text
Feroxbuster
     ↓
Interesting Endpoint
     ↓
Burp Repeater
     ↓
Manual Analysis
```

Example:

```text
/api/admin
/debug
/internal
```

The endpoint name alone is not a finding.

---

# Feroxbuster + Caido

```text
Feroxbuster
     ↓
Discovery
     ↓
Caido
     ↓
Replay / Analyze
```

---

# Feroxbuster + httpx

Large-scope workflow:

```text
Subfinder / Amass
       ↓
httpx
       ↓
Live Hosts
       ↓
Feroxbuster
       ↓
Content Discovery
```

Apply conservative concurrency when working across many hosts.

---

# Feroxbuster vs FFUF

Both discover web content.

### FFUF

```text
Flexible FUZZ insertion
```

Useful for:

```text
Directories
Parameters
VHosts
Headers
Request bodies
```

### Feroxbuster

Focused strongly on:

```text
Recursive web-content discovery
```

Mental model:

```text
FFUF
=
Flexible Fuzzer

Feroxbuster
=
Recursive Content Discovery
```

---

# Feroxbuster vs Gobuster

Gobuster can enumerate:

```text
Directories
DNS names
Virtual hosts
```

Feroxbuster emphasizes:

```text
Recursive web discovery
```

The tools overlap but have different workflow strengths.

---

# Robots.txt

Always inspect:

```text
/robots.txt
```

before aggressive discovery.

It may contain paths such as:

```text
Disallow: /admin/
Disallow: /internal/
```

Important:

```text
robots.txt
!=
Access Control
```

It only provides crawler instructions.

Sensitive paths still require real authentication and authorization.

---

# Sitemap

Also inspect:

```text
/sitemap.xml
```

It may reveal legitimate application URLs.

Using known application metadata can reduce unnecessary brute-force traffic.

---

# JavaScript

Modern web applications expose routes/endpoints in JavaScript.

Therefore a broader methodology combines:

```text
Content brute force
       +
robots.txt
       +
sitemap.xml
       +
JavaScript analysis
       +
Historical URLs
```

rather than relying only on one wordlist.

---

# False Positives

Common causes include:

```text
Custom error pages
Wildcard routing
Authentication redirects
WAF responses
Rate limiting
Identical SPA responses
```

Always validate manually.

---

# Content Discovery Methodology

```text
1. Browse application

2. Check robots.txt

3. Check sitemap.xml

4. Identify technology

5. Establish error baseline

6. Choose wordlist

7. Start discovery

8. Review 2xx / 3xx / 401 / 403

9. Follow useful recursion

10. Validate endpoints manually
```

---

# Output Management

Large scans can generate many findings.

Organize by:

```text
Host
Path
Status
Size
Content Type
Notes
```

Useful conceptual pipeline:

```text
Discovery
   ↓
Filter
   ↓
Prioritize
   ↓
Manual Validation
```

---

# Security Questions

For each interesting resource ask:

```text
What is this endpoint?

Is authentication required?

Is authorization correctly enforced?

Does it expose sensitive information?

Is it intended to be publicly reachable?

Does it reveal additional functionality?
```

---

# Important Principle

Finding:

```text
/admin/
```

does not mean:

```text
Admin panel compromised
```

Finding:

```text
/config/
```

does not mean:

```text
Credentials exposed
```

Discovery identifies **attack surface**.

Manual validation determines security impact.

---

# Recommended Tool Chain

```text
Amass / Subfinder
       ↓
Subdomains
       ↓
httpx
       ↓
Live Web Apps
       ↓
Feroxbuster
       ↓
Hidden Content
       ↓
Burp / Caido
       ↓
Manual Testing
```

---

# Key Takeaway

Feroxbuster's mental model is:

```text
WEB ROOT
   ↓
DISCOVER PATH
   ↓
RECURSE
   ↓
DISCOVER DEEPER PATH
   ↓
FILTER
   ↓
VALIDATE
```

Its strength is:

```text
Fast
+
Recursive
+
Automated
```

But the rule remains:

```text
Discovered Resource
       !=
Vulnerability
```

Always finish with:

```text
Manual Validation
```
