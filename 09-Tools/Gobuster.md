# Gobuster

> Gobuster fundamentals for authorized directory, DNS, and virtual-host enumeration.

## What is Gobuster?

**Gobuster** is a fast enumeration tool written in Go.

It uses wordlists to discover resources such as:

```text
Directories
Files
DNS subdomains
Virtual hosts
```

Conceptually:

```text
Wordlist
   |
Gobuster
   |
   v
Candidate Requests
   |
   v
Discovered Resources
```

---

# Common Modes

Depending on version, common Gobuster modes include:

```text
dir
dns
vhost
```

Always check:

```bash
gobuster -h
```

and:

```bash
gobuster <mode> -h
```

for current syntax.

---

# Directory Mode

Directory mode performs web-content discovery.

Conceptually:

```text
Wordlist
 |
 +-- admin
 +-- login
 +-- api
 +-- backup
 |
 v
Web Server
```

A common authorized-lab pattern:

```bash
gobuster dir -u https://example.test -w wordlist.txt
```

---

# How Directory Discovery Works

Given:

```text
admin
api
images
backup
```

Gobuster requests candidate paths such as:

```text
/admin
/api
/images
/backup
```

The server's responses help determine which resources may exist.

---

# Wordlists

Wordlist choice strongly affects results.

Common categories:

```text
Directories
Files
Technology-specific paths
API routes
Backup names
Administrative paths
```

Collections such as:

```text
SecLists
```

are commonly used in authorized labs.

---

# Status Codes

Important results include:

```text
200 OK
204 No Content

301 Moved
302 Found

401 Unauthorized
403 Forbidden

404 Not Found
```

Do not automatically ignore:

```text
401
403
```

because they may identify real protected resources.

---

# 403 Meaning

Example:

```text
/admin -> 403
```

This may indicate:

```text
Resource exists
      +
Current request forbidden
```

It does **not** mean:

```text
Authentication bypass exists
```

Manual analysis is required.

---

# Redirects

A directory might return:

```text
301
```

and redirect:

```text
/admin
```

to:

```text
/admin/
```

Therefore redirects can be useful discovery signals.

---

# File Extensions

Applications may use:

```text
.php
.aspx
.jsp
.html
.txt
.json
```

Gobuster directory mode supports extension-aware workflows.

Before adding many extensions, fingerprint the target technology.

---

# Baseline Response

Before enumeration, request a clearly nonexistent resource.

Example:

```text
/random-path-that-does-not-exist
```

Observe:

```text
Status
Response size
Redirect
Content
```

Why?

Because some applications return:

```text
200 OK
```

for every path.

---

# Custom 404

Example:

```text
/not-real
     ↓
200 OK
     ↓
"Sorry, page not found"
```

A brute-force tool may interpret this as a valid page unless response behavior is accounted for.

Therefore:

```text
Understand Baseline
      ↓
Configure Enumeration
```

---

# DNS Mode

Gobuster can perform DNS subdomain enumeration.

Conceptually:

```text
Wordlist
 |
 +-- www
 +-- api
 +-- dev
 +-- vpn
 |
 v
DNS
 |
 v
www.example.test
api.example.test
...
```

A common conceptual pattern:

```bash
gobuster dns -d example.test -w subdomains.txt
```

Use only against domains within scope.

---

# DNS Brute Force vs Passive Discovery

Gobuster DNS:

```text
Candidate generation
      +
DNS queries
```

Passive tools:

```text
Subfinder
Amass passive mode
Assetfinder
```

primarily leverage existing external datasets.

Therefore:

```text
Passive Discovery
       +
Active DNS Enumeration
       =
Broader Coverage
```

when active testing is authorized.

---

# Wildcard DNS

Some domains resolve every hostname.

Example:

```text
random123.example.test
random456.example.test
anything.example.test
```

all resolve to the same address.

This is:

```text
Wildcard DNS
```

It can create large numbers of false positives.

Test a random hostname before interpreting DNS brute-force results.

---

# VHost Mode

**VHost = Virtual Host**

One web server/IP can host multiple applications.

Example:

```text
203.0.113.10
 |
 +-- www.example.test
 +-- admin.example.test
 +-- dev.example.test
```

HTTP selects the application partly using:

```http
Host: ...
```

---

# Virtual Host Discovery

Conceptually:

```text
Wordlist
   |
   +-- admin
   +-- dev
   +-- portal
   |
   v
Host Header
   |
   v
Web Server
```

Different responses can reveal additional applications.

---

# DNS vs VHost

Important distinction:

```text
DNS Subdomain
```

and:

```text
HTTP Virtual Host
```

are related but not identical.

A virtual host may be configured on a server even when public DNS does not expose it.

---

# Response Length

When virtual-host discovery returns the same status code for everything, compare:

```text
Response length
Words
Headers
Content
```

Example:

```text
random.example.test -> 200 / 5000 bytes

admin.example.test  -> 200 / 8200 bytes
```

The difference is a lead requiring validation.

---

# Threads

Gobuster supports concurrent requests.

Conceptually:

```text
More Threads
    ↓
More Requests / Second
    ↓
Faster Enumeration
```

but:

```text
More Load
More Logs
More Rate Limiting
```

Do not maximize concurrency automatically.

---

# Rate and Scope

Content enumeration is active testing.

Respect:

```text
Rules of engagement
Application capacity
Rate limits
WAF behavior
Scope
```

---

# Authenticated Enumeration

Some application content may require an authenticated session.

Where authorized, enumeration tools may be configured with appropriate:

```text
Cookies
Headers
Tokens
```

Treat these values as credentials.

Do not expose them in:

```text
Git
Screenshots
Shared shell history
Reports unnecessarily
```

---

# Gobuster + Burp

Workflow:

```text
Gobuster
    ↓
Interesting Path
    ↓
Burp Repeater
    ↓
Manual Analysis
```

Example:

```text
/admin
/api
/debug
```

The discovered path itself is not automatically a vulnerability.

---

# Gobuster + Caido

Same model:

```text
Gobuster
   ↓
Endpoint
   ↓
Caido
   ↓
Inspect / Replay
```

---

# Gobuster + httpx

Large-scope workflow:

```text
Subdomains
    ↓
httpx
    ↓
Live HTTP Hosts
    ↓
Gobuster
    ↓
Content Discovery
```

Be cautious about multiplying request volume across many hosts.

---

# Gobuster vs FFUF

Both can perform content discovery.

```text
Gobuster
 |
Dedicated enumeration modes
 |
dir / dns / vhost
```

while:

```text
FFUF
 |
Flexible FUZZ insertion
 |
Paths / headers / parameters / bodies
```

FFUF is generally more flexible as a generic HTTP fuzzer.

---

# Gobuster vs Feroxbuster

```text
Gobuster
 |
Fast directory/DNS/VHost enumeration
```

```text
Feroxbuster
 |
Strong recursive content discovery
```

Feroxbuster often emphasizes automatic recursion.

---

# Gobuster vs Subfinder

```text
Gobuster DNS
      |
Active candidate testing
```

```text
Subfinder
      |
Passive subdomain discovery
```

A typical workflow uses passive reconnaissance first.

---

# robots.txt

Before large directory enumeration, inspect:

```text
/robots.txt
```

It may reveal paths.

Example:

```text
Disallow: /admin/
```

Remember:

```text
robots.txt != Authorization
```

It is crawler guidance, not a security boundary.

---

# sitemap.xml

Also inspect:

```text
/sitemap.xml
```

This may expose legitimate application paths without brute forcing.

---

# Enumeration Methodology

```text
1. Define scope

2. Fingerprint application

3. Check robots.txt

4. Check sitemap.xml

5. Establish error baseline

6. Select wordlist

7. Start conservative enumeration

8. Review 2xx / 3xx / 401 / 403

9. Validate interesting paths

10. Perform deeper enumeration only where useful
```

---

# False Positives

Common causes:

```text
Wildcard DNS
Custom 404
SPA routing
Authentication redirects
WAF pages
Rate limiting
Same response for all VHosts
```

Always manually validate.

---

# Key Takeaway

Gobuster provides several important discovery models:

```text
DIR
 |
Hidden web content

DNS
 |
Subdomain candidates

VHOST
 |
HTTP virtual hosts
```

The workflow should always be:

```text
Wordlist
   ↓
Discovery
   ↓
Filter
   ↓
Scope Check
   ↓
Manual Validation
```

Remember:

```text
Found Resource
     !=
Vulnerability
```
