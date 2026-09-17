# Wordlists

> Wordlist and payload-reference fundamentals for authorized reconnaissance, content discovery, password auditing, and application-security testing.

## Module Overview

A **wordlist** is a collection of candidate values used by security tools to systematically test or discover something.

Examples:

```text
Directories
Files
Subdomains
Parameters
Usernames
Passwords
API endpoints
VHosts
Payload patterns
```

Conceptually:

```text
Wordlist
   ↓
Security Tool
   ↓
Candidate Input
   ↓
Target
   ↓
Response
```

---

# Files

```text
Assetnote.md
OneListForAll.md
PayloadsAllTheThings.md
README.md
SecLists.md
```

---

# Why Wordlists Matter

Many security tools require candidate input.

Examples:

```text
FFUF
Gobuster
Feroxbuster
Hashcat
John the Ripper
```

Without a useful candidate list:

```text
Tool
 ↓
Poor Input
 ↓
Poor Coverage
```

Therefore:

```text
Tool Quality
     +
Wordlist Quality
     =
Better Enumeration
```

---

# Wordlist Categories

Common categories include:

```text
Web Content
DNS / Subdomains
VHosts
Usernames
Passwords
Parameters
API Routes
File Extensions
Fuzzing Inputs
```

Different tasks require different lists.

---

# Web Content Wordlists

Used to discover:

```text
Directories
Files
Endpoints
Administrative paths
Backup files
API routes
```

Example:

```text
admin
login
api
dashboard
uploads
backup
```

A tool may test:

```text
/admin
/login
/api
/dashboard
/uploads
/backup
```

---

# DNS Wordlists

Used for candidate subdomain enumeration.

Example:

```text
www
api
mail
dev
vpn
portal
```

Combined with:

```text
example.com
```

produces:

```text
www.example.com
api.example.com
mail.example.com
dev.example.com
```

---

# Password Wordlists

Password-auditing tools can consume lists containing candidate passwords.

Used with:

```text
Hashcat
John the Ripper
```

in authorized offline password audits.

Conceptually:

```text
Password Candidates
        ↓
Hash Function / KDF
        ↓
Compare Against Hash
```

---

# Username Wordlists

Username lists can support controlled identity/security testing.

Possible formats:

```text
alice
bob
john.smith
jsmith
```

Real-user enumeration and authentication testing require explicit authorization.

---

# Parameter Wordlists

Web applications may contain undocumented parameters.

Examples:

```text
id
user
page
debug
lang
redirect
```

Conceptually:

```text
/api?FUZZ=test
```

A fuzzing tool can replace:

```text
FUZZ
```

with candidate parameter names.

---

# API Wordlists

Modern applications frequently expose:

```text
/api
/api/v1
/graphql
/swagger
/openapi.json
/health
/status
```

API-specific lists can provide better coverage than generic directory lists.

---

# Assetnote

Assetnote provides data-driven wordlists useful for web-content discovery.

Conceptually:

```text
Real-World Web Data
       ↓
Observed Paths
       ↓
Wordlists
```

Useful with:

```text
FFUF
Feroxbuster
Gobuster
```

---

# OneListForAll

OneListForAll provides a broad consolidated web-content wordlist.

Useful for:

```text
Directories
Files
Common web paths
Broad content discovery
```

Its size can increase coverage but also generates more traffic.

---

# PayloadsAllTheThings

PayloadsAllTheThings differs from a traditional path wordlist.

It provides:

```text
Security-testing references
Payload patterns
Vulnerability-class notes
Testing techniques
```

Topics include areas such as:

```text
XSS
SQL Injection
SSRF
SSTI
Path Traversal
Command Injection
XXE
JWT
File Upload
```

It should be used as a **reference**, not blindly sprayed against applications.

---

# SecLists

**SecLists** is a large collection of security-testing lists.

It includes categories for:

```text
Discovery
DNS
Web Content
Passwords
Usernames
Fuzzing
Payloads
Pattern Matching
```

It is one of the most commonly used wordlist collections in security labs.

---

# Wordlist Selection

Do not automatically choose:

```text
Largest List Available
```

Instead determine:

```text
What am I testing?
```

Then choose:

```text
Relevant Category
       ↓
Relevant Technology
       ↓
Appropriate Size
```

---

# Small vs Large Lists

## Small List

Advantages:

```text
Fast
Low traffic
Quick initial coverage
```

Disadvantage:

```text
May miss uncommon resources
```

## Large List

Advantages:

```text
Broader candidate coverage
```

Disadvantages:

```text
More requests
More runtime
More server load
More noise
```

---

# Progressive Enumeration

A good strategy:

```text
Small Relevant List
       ↓
Review Results
       ↓
Target-Specific List
       ↓
Medium List
       ↓
Large List if Required
```

This is usually more efficient than immediately sending millions of requests.

---

# Target-Specific Wordlists

Generic lists are useful, but target-specific lists can be much more effective.

Sources include:

```text
Katana
GAU
Waybackurls
JavaScript
API documentation
Application terminology
robots.txt
sitemap.xml
```

Conceptually:

```text
Target Data
    ↓
Extract Paths / Words
    ↓
Custom Wordlist
```

---

# Current URLs

Use:

```text
Katana
```

to discover currently linked/crawlable content.

```text
Application
    ↓
Katana
    ↓
Current Paths
```

---

# Historical URLs

Use:

```text
GAU
Waybackurls
```

to discover previously observed paths.

```text
Historical Sources
       ↓
Old URLs
       ↓
Potential Candidates
```

---

# Generic + Target-Specific

Best coverage often comes from:

```text
Generic Wordlist
       +
Current URLs
       +
Historical URLs
       +
Application Vocabulary
       ↓
Custom Dataset
```

---

# Combining Wordlists

Example:

```bash
cat list1.txt list2.txt list3.txt |
sort -u > combined.txt
```

This:

```text
Merges
+
Deduplicates
```

the lists.

---

# Normalize Before Use

Lists may contain:

```text
Duplicates
Blank lines
Comments
Different formats
Unexpected prefixes
```

Inspect and normalize them before large scans.

---

# FFUF

Wordlist:

```text
admin
api
login
```

Request pattern:

```text
https://example.test/FUZZ
```

Conceptually:

```text
Wordlist
 ↓
FFUF
 ↓
HTTP Requests
```

---

# Gobuster

Web-content discovery:

```text
Wordlist
 ↓
Gobuster
 ↓
Candidate Paths
```

Gobuster can also use appropriate lists for:

```text
DNS
VHost discovery
```

---

# Feroxbuster

Useful with directory/file lists for:

```text
Recursive Content Discovery
```

Conceptually:

```text
Wordlist
 ↓
Discover /api
 ↓
Recurse
 ↓
Discover /api/v1
```

---

# Hashcat

Password candidate list:

```text
Wordlist
 ↓
Rules
 ↓
Candidate Passwords
 ↓
Offline Hash Audit
```

Password wordlists and web-content wordlists serve completely different purposes.

---

# John the Ripper

Similarly:

```text
Password Wordlist
      ↓
John
      ↓
Offline Password Verification
```

Rules can mutate base words into additional candidates.

---

# Baseline Before Fuzzing

Before large web-content discovery, request a clearly nonexistent path.

Example:

```text
/random-invalid-12345
```

Observe:

```text
Status
Size
Words
Lines
Redirect
Content
```

This helps identify false positives.

---

# Custom 404

Some applications return:

```text
200 OK
```

for every path.

Example:

```text
/not-real
   ↓
200
   ↓
"Page Not Found"
```

Without filtering:

```text
Every Word
   ↓
Looks Valid
```

Always understand baseline behavior.

---

# Status Codes

Interesting results can include:

```text
200
204
301
302
401
403
```

Remember:

```text
401 / 403
```

may indicate a real protected resource.

But:

```text
Protected Resource
      !=
Vulnerability
```

---

# Extensions

Applications may use:

```text
.php
.aspx
.jsp
.html
.json
.xml
.txt
```

Only test relevant extensions.

Otherwise:

```text
Wordlist Size
    ×
Extensions
    =
Huge Request Count
```

---

# Request Explosion

Example:

```text
500,000 words
×
10 extensions
×
20 hosts
=
100,000,000 requests
```

before recursion.

Therefore control:

```text
Wordlist size
Extensions
Hosts
Concurrency
Recursion
Rate
```

---

# Rate Control

Wordlist-based testing can trigger:

```text
Rate limiting
WAF blocks
Server load
Large logs
Temporary bans
```

Use conservative rates according to the rules of engagement.

---

# Wordlists Are Untrusted Input

Downloaded lists may contain unexpected:

```text
Characters
Encodings
Very long entries
Malformed values
```

Inspect unfamiliar lists before feeding them into automation.

---

# Payload Lists

Payload collections require even more care.

Some payloads can trigger:

```text
Errors
External requests
File operations
Database behavior
Application instability
```

Understand a payload before using it.

---

# Discovery != Vulnerability

Finding:

```text
/admin
```

does not mean:

```text
Admin compromise
```

Finding:

```text
/backup
```

does not mean:

```text
Sensitive backup exposed
```

Correct flow:

```text
Discovery
   ↓
Manual Validation
   ↓
Security Impact
```

---

# Wordlist Methodology

```text
1. Define authorized scope

2. Fingerprint target

3. Crawl current application

4. Collect historical URLs

5. Build target-specific candidates

6. Select relevant public list

7. Establish error baseline

8. Start conservative discovery

9. Filter noise

10. Manually validate results
```

---

# Tool + Wordlist Map

```text
FFUF
   ↓
Web / parameter / VHost lists

Gobuster
   ↓
Directory / DNS / VHost lists

Feroxbuster
   ↓
Web-content lists

Hashcat
   ↓
Password candidate lists

John
   ↓
Password candidate lists
```

---

# Key Takeaway

A wordlist is simply:

```text
CANDIDATE INPUT
```

Its usefulness depends on:

```text
RELEVANCE
   +
QUALITY
   +
TARGET CONTEXT
   +
TOOL CONFIGURATION
```

The best methodology is not:

```text
Largest Wordlist
       +
Maximum Threads
```

It is:

```text
Understand Target
       ↓
Choose Relevant Candidates
       ↓
Control Request Volume
       ↓
Filter Responses
       ↓
Validate Manually
```
