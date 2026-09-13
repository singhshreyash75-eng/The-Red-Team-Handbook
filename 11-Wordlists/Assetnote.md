# Assetnote Wordlists

> Assetnote wordlist fundamentals for authorized web-content discovery, reconnaissance, and attack-surface enumeration.

## What is Assetnote?

**Assetnote** is a security research company known for attack-surface management research and high-quality security wordlists.

Its publicly available wordlists are commonly used for:

```text
Directory Discovery
File Discovery
API Endpoint Discovery
Web Content Enumeration
Technology-Specific Enumeration
```

Conceptually:

```text
Real-World Web Data
       ↓
Wordlist Generation
       ↓
Assetnote Wordlists
       ↓
Content Discovery Tools
```

---

# Why Wordlists Matter

Content-discovery tools such as:

```text
FFUF
Feroxbuster
Gobuster
```

need candidate paths.

Example:

```text
Wordlist

admin
api
login
dashboard
backup
```

Tool tests:

```text
/admin
/api
/login
/dashboard
/backup
```

Therefore:

```text
Discovery Quality
      depends partly on
Wordlist Quality
```

---

# Traditional vs Data-Driven Wordlists

Traditional wordlists may contain manually collected/common names:

```text
admin
backup
login
test
dev
```

Data-driven wordlists are generated using large amounts of observed web data.

Conceptually:

```text
Real Web Applications
       ↓
Observed Paths
       ↓
Frequency / Processing
       ↓
Practical Wordlists
```

Assetnote wordlists follow this broader data-driven philosophy.

---

# Common Use Cases

Assetnote wordlists can help with:

```text
Directories
Files
API routes
Technology-specific paths
Common web resources
```

They are particularly useful during:

```text
Web Enumeration
Bug Bounty Recon
Attack-Surface Testing
Pentesting Labs
```

---

# Assetnote + FFUF

Typical authorized workflow:

```text
Target Application
       ↓
Assetnote Wordlist
       ↓
FFUF
       ↓
Candidate Paths
       ↓
Interesting Responses
```

Conceptual command:

```bash
ffuf -w wordlist.txt -u https://example.test/FUZZ
```

---

# Assetnote + Feroxbuster

```text
Assetnote Wordlist
       ↓
Feroxbuster
       ↓
Recursive Discovery
```

Example structure:

```bash
feroxbuster -u https://example.test -w wordlist.txt
```

Feroxbuster can continue exploring discovered directories.

---

# Assetnote + Gobuster

Conceptually:

```bash
gobuster dir \
-u https://example.test \
-w wordlist.txt
```

Workflow:

```text
Wordlist
   ↓
Gobuster
   ↓
Web Server
   ↓
Responses
```

---

# Wordlist Size

Larger wordlists provide:

```text
More Candidates
```

but also:

```text
More Requests
Longer Runtime
More Logs
Higher Server Load
More Noise
```

Therefore:

```text
Largest Wordlist
      !=
Best Wordlist
```

---

# Choose Based on Target

Before selecting a wordlist, determine:

```text
Technology
Application type
Server/framework
API structure
Known paths
Engagement size
```

Example:

```text
WordPress Application
      ↓
WordPress-Relevant Candidates
```

is generally more efficient than blindly testing millions of unrelated paths.

---

# Response Baseline

Before content discovery, request a clearly nonexistent path.

Example:

```text
/random-path-that-should-not-exist
```

Observe:

```text
Status Code
Content Length
Words
Redirect
Body
```

This creates a baseline.

---

# Custom 404 Problem

Some applications return:

```text
200 OK
```

for nonexistent pages.

Example:

```text
/random123
     ↓
200 OK
     ↓
"Page Not Found"
```

A wordlist scanner can therefore generate thousands of false positives.

Use filtering based on:

```text
Size
Words
Lines
Content
```

---

# Status Codes

Interesting results commonly include:

```text
200
204
301
302
401
403
```

Do not assume:

```text
403 = useless
```

A 403 response may indicate a real protected resource.

But:

```text
403
!=
Vulnerability
```

---

# Technology-Specific Wordlists

One advantage of specialized wordlists is reducing unnecessary requests.

Examples conceptually:

```text
Apache
IIS
Java
PHP
WordPress
API
Cloud applications
```

Workflow:

```text
Fingerprint Target
       ↓
Select Relevant Wordlist
       ↓
Enumerate
```

---

# Custom Wordlists

Public wordlists should not be the only source.

A target-specific list can be generated from:

```text
Katana results
GAU results
Waybackurls
JavaScript
Application terminology
Public documentation
```

Conceptually:

```text
Target-Specific Data
        ↓
Extract Words / Paths
        ↓
Custom Wordlist
```

---

# Assetnote + Katana

Katana finds currently referenced paths:

```text
Katana
 ↓
Current URLs
```

Assetnote wordlists test additional candidates:

```text
Assetnote
 ↓
Potential Hidden URLs
```

Together:

```text
Known
+
Guessed
=
Broader Coverage
```

---

# Assetnote + Historical URLs

Historical reconnaissance:

```text
GAU
Waybackurls
```

can reveal previously existing paths.

Combine:

```text
Historical Paths
       +
Assetnote Candidates
       ↓
Improved Enumeration
```

---

# API Enumeration

Modern applications frequently expose paths such as:

```text
/api
/api/v1
/api/users
/graphql
/swagger
/openapi.json
```

API-focused wordlists can help locate undocumented endpoints.

Finding an endpoint does not mean it is vulnerable.

---

# Wordlist Deduplication

If combining lists:

```bash
cat list1.txt list2.txt |
sort -u > combined.txt
```

This prevents duplicate requests.

---

# Prioritization

A good strategy:

```text
Small Relevant List
       ↓
Review Results
       ↓
Medium List
       ↓
Target-Specific List
       ↓
Large List if Required
```

instead of immediately starting with the largest available dataset.

---

# Rate Control

Suppose:

```text
Wordlist = 1,000,000 entries
```

against:

```text
100 hosts
```

That can potentially mean:

```text
100,000,000 requests
```

before recursion or extensions.

Therefore control:

```text
Rate
Concurrency
Targets
Extensions
Recursion
```

---

# Scope

A wordlist does not determine authorization.

Always ensure:

```text
Target
 ↓
In Scope?
 ↓
Yes
 ↓
Enumerate
```

Do not automatically follow discovered third-party URLs.

---

# False Positives

Common causes:

```text
Custom 404 pages
Wildcard routing
Authentication redirects
WAF responses
Rate limiting
SPA fallback pages
```

Always manually validate important results.

---

# Recommended Workflow

```text
Target
  ↓
Fingerprint Technology
  ↓
Check robots.txt / sitemap
  ↓
Crawl with Katana
  ↓
Historical URLs
  ↓
Choose Relevant Wordlist
  ↓
FFUF / Feroxbuster / Gobuster
  ↓
Filter Results
  ↓
Burp / Caido
  ↓
Manual Validation
```

---

# Assetnote vs SecLists

Both provide useful wordlists.

Conceptually:

```text
Assetnote
    |
Data-driven web discovery lists
```

```text
SecLists
    |
Large multi-purpose security
testing collection
```

They complement each other.

---

# Key Takeaway

Assetnote wordlists provide:

```text
REAL-WORLD DATA
       ↓
USEFUL CANDIDATES
       ↓
WEB CONTENT DISCOVERY
```

But wordlists should be selected intelligently.

Use:

```text
TARGET KNOWLEDGE
      +
RELEVANT WORDLIST
      +
RATE CONTROL
      +
MANUAL VALIDATION
```

rather than:

```text
Biggest List
   ↓
Maximum Threads
   ↓
Hope for Results
```
