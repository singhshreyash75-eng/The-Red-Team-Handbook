# SecLists

> SecLists fundamentals for authorized reconnaissance, web discovery, fuzzing, username testing, and offline password auditing.

## What is SecLists?

**SecLists** is a large collection of security-testing wordlists maintained as an open-source project.

It organizes candidate data for many security-testing tasks.

Common categories include:

```text
Discovery
Web Content
DNS
Fuzzing
Passwords
Usernames
Pattern Matching
Payload-related test data
```

Conceptually:

```text
              SecLists
                 |
      +----------+----------+
      |          |          |
  Discovery   Passwords   Fuzzing
      |          |          |
      v          v          v
    FFUF      Hashcat     Testing
 Gobuster      John
```

---

# Why SecLists is Useful

Without SecLists, testers often need to create candidate lists manually.

SecLists provides organized collections for common tasks.

Instead of:

```text
Create thousands of candidates manually
```

use:

```text
Relevant SecLists category
        ↓
Security Tool
```

---

# Installation

On some Kali/Debian-based security environments, SecLists may be available through the package manager.

A typical package installation is:

```bash
sudo apt install seclists
```

The exact installation path depends on the environment.

A commonly encountered Kali path is:

```text
/usr/share/seclists/
```

Check locally rather than assuming.

---

# Repository Structure

The exact repository evolves, but major categories commonly include areas such as:

```text
Discovery/
Fuzzing/
Passwords/
Usernames/
Pattern-Matching/
Payloads/
```

Explore:

```bash
find /usr/share/seclists -maxdepth 2 -type d
```

when installed there.

---

# Discovery

The:

```text
Discovery
```

category contains lists useful for discovering resources.

Examples:

```text
Web Content
DNS
Infrastructure-related candidates
```

---

# Web Content Discovery

Web lists contain candidate:

```text
Directories
Files
Endpoints
Common resources
```

Example entries:

```text
admin
login
api
dashboard
uploads
backup
```

Used with:

```text
FFUF
Gobuster
Feroxbuster
```

---

# FFUF + SecLists

Conceptual authorized usage:

```bash
ffuf \
-w /path/to/seclists/web-list.txt \
-u https://example.test/FUZZ
```

Flow:

```text
SecLists
   ↓
FFUF
   ↓
Candidate Paths
   ↓
HTTP Responses
```

Select the actual installed list appropriate for the target rather than copying a path blindly.

---

# Gobuster + SecLists

Conceptually:

```bash
gobuster dir \
-u https://example.test \
-w /path/to/wordlist.txt
```

Use a directory/file list matching the application.

---

# Feroxbuster + SecLists

Conceptually:

```bash
feroxbuster \
-u https://example.test \
-w /path/to/wordlist.txt
```

Feroxbuster can then recursively explore discovered directories.

---

# DNS Discovery

SecLists contains candidate names useful for authorized DNS enumeration.

Example:

```text
www
api
mail
dev
stage
vpn
portal
```

Conceptually:

```text
Candidate
    +
example.com
    ↓
api.example.com
```

---

# Gobuster DNS

Conceptually:

```bash
gobuster dns \
-d example.test \
-w subdomains.txt
```

This performs active DNS candidate testing.

Compare:

```text
SecLists + Gobuster
      ↓
Active DNS Enumeration
```

with:

```text
Subfinder
      ↓
Passive Discovery
```

---

# Passive First

A sensible recon methodology:

```text
Subfinder / Amass
       ↓
Known Subdomains
       ↓
Validate
       ↓
Wordlist-Based DNS Enumeration
if required and authorized
```

This reduces unnecessary DNS traffic.

---

# VHost Discovery

SecLists candidates can also support virtual-host enumeration.

Conceptually:

```text
Candidate Word
      ↓
Host Header
      ↓
Web Server
      ↓
Different Response?
```

Virtual hosts may exist even when public DNS does not directly expose them.

---

# Web vs DNS Lists

Do not use lists interchangeably without understanding them.

```text
Web Content List
=
admin/
login/
api/
```

```text
DNS List
=
www
mail
vpn
dev
```

The optimal structure differs by task.

---

# Password Lists

SecLists includes password candidate collections useful for authorized password-security auditing.

Use primarily with offline auditing tools such as:

```text
Hashcat
John the Ripper
```

Conceptually:

```text
Password Candidates
        ↓
Offline Audit
        ↓
Password Strength Assessment
```

---

# Password Auditing

With authorized hash material:

```text
Hash
  +
SecLists Password Candidates
  ↓
Hashcat / John
  ↓
Offline Verification
```

The goal should be:

```text
Assess Password Resilience
```

not collecting unnecessary plaintext credentials.

---

# Online Authentication Warning

Password lists can technically be fed into online authentication tools, but doing so creates:

```text
Lockout risk
High request volume
Authentication alerts
Service impact
```

Online password testing requires explicit authorization, defined test accounts, and rate controls.

---

# Username Lists

SecLists also contains username-related lists.

Examples conceptually:

```text
admin
administrator
guest
service
test
```

These can support authorized:

```text
Default-account review
Lab testing
Controlled authentication assessment
```

---

# Usernames vs Real Users

A generic username candidate list is different from:

```text
A harvested list of real employee accounts
```

Real identity information may be sensitive and should be handled according to engagement requirements.

---

# Fuzzing Lists

The fuzzing category contains test inputs intended to exercise application parsers and edge cases.

Potential categories include:

```text
Special characters
Encoding cases
Protocol-specific values
Application test strings
```

Fuzzing can cause unexpected behavior.

Use relevant subsets rather than blindly sending everything.

---

# Fuzzing vs Content Discovery

Important distinction:

```text
Content Wordlist
       ↓
"What path exists?"
```

versus:

```text
Fuzzing Input
       ↓
"How does this input behave?"
```

These are different testing objectives.

---

# Parameter Discovery

Candidate lists can help identify undocumented parameters.

Conceptually:

```text
/api?FUZZ=test
```

Candidate list:

```text
id
user
page
lang
debug
```

Possible requests:

```text
/api?id=test
/api?user=test
/api?page=test
```

Response differences require manual interpretation.

---

# API Discovery

Modern API targets may benefit from lists containing candidates such as:

```text
api
v1
v2
users
accounts
health
status
docs
swagger
graphql
```

Technology and application context should guide selection.

---

# File Extensions

Web discovery may also test relevant extensions:

```text
.php
.aspx
.jsp
.json
.xml
.txt
```

Do not combine every extension with every huge list.

Request counts multiply quickly.

---

# Request Count

Suppose:

```text
200,000 words
×
8 extensions
×
10 hosts
```

That already creates approximately:

```text
16,000,000 candidate combinations
```

before recursion.

Therefore plan before scanning.

---

# Small Lists First

A practical strategy:

```text
Small / Common List
        ↓
Review
        ↓
Technology-Specific List
        ↓
Target-Specific List
        ↓
Large List if Needed
```

This reduces noise and improves efficiency.

---

# Technology-Specific Lists

If the application is known to use:

```text
IIS
Apache
PHP
Java
WordPress
API framework
```

select candidates relevant to that technology.

Conceptually:

```text
Fingerprint
    ↓
Relevant Wordlist
    ↓
Higher Signal
```

---

# Custom Wordlists

SecLists should complement—not replace—target-specific discovery.

Build custom candidates from:

```text
Katana
GAU
Waybackurls
JavaScript
robots.txt
sitemap.xml
API documentation
Application terminology
```

---

# Combining Lists

Example:

```bash
cat target-specific.txt public-list.txt |
sort -u > combined.txt
```

Conceptually:

```text
Target Knowledge
       +
SecLists
       ↓
Combined Candidate Set
```

---

# Deduplication

Always remove duplicate entries when merging:

```bash
sort -u combined.txt -o combined.txt
```

This reduces unnecessary requests.

---

# Case Sensitivity

Some applications/filesystems distinguish:

```text
/admin
```

from:

```text
/Admin
```

Others do not.

Wordlist case variations can increase coverage but also multiply requests.

Understand the target platform first.

---

# Baseline Response

Before directory fuzzing:

```text
Request Random Invalid Path
        ↓
Observe Response
```

Record:

```text
Status
Length
Words
Redirect
Content
```

Then configure filtering.

---

# False Positives

Common causes include:

```text
Custom 404 pages
Wildcard DNS
SPA fallback routing
Authentication redirects
WAF responses
Rate limiting
Catch-all VHosts
```

A wordlist tool does not automatically understand application semantics.

---

# 401 / 403 Results

Do not discard:

```text
401
403
```

automatically.

They may indicate valid protected resources.

But:

```text
Protected Resource
      !=
Authorization Bypass
```

Manual validation is still required.

---

# SecLists + Burp / Caido

A good workflow:

```text
SecLists
   ↓
FFUF / Feroxbuster
   ↓
Interesting Endpoint
   ↓
Burp / Caido
   ↓
Manual Analysis
```

Automation discovers.

The proxy validates.

---

# SecLists + Nuclei

These serve different purposes.

```text
SecLists
    |
Candidate input
```

```text
Nuclei
    |
Template-based checks
```

Example workflow:

```text
SecLists
   ↓
Discover Endpoint
   ↓
Nuclei / Manual Testing
```

---

# SecLists vs Assetnote

```text
SecLists
    |
Broad multi-category collection
```

```text
Assetnote
    |
Data-driven web-discovery lists
```

Use both when appropriate.

---

# SecLists vs OneListForAll

```text
SecLists
    |
Many categories and specialized lists
```

```text
OneListForAll
    |
Broad consolidated web-content list
```

OneListForAll is more focused, while SecLists functions as a larger toolbox of lists.

---

# SecLists vs PayloadsAllTheThings

```text
SecLists
    |
Candidate data / wordlists
```

```text
PayloadsAllTheThings
    |
Vulnerability-testing reference
and payload patterns
```

There is some conceptual overlap, but their main purposes differ.

---

# Wordlist Selection Checklist

Before selecting a list ask:

```text
What am I discovering?

Web path?
Subdomain?
VHost?
Parameter?
Username?
Password?
API route?
```

Then ask:

```text
What technology?

How many targets?

What traffic is acceptable?

What baseline response exists?

How will results be validated?
```

---

# Operational Safety

Large wordlists can generate substantial traffic.

Control:

```text
Concurrency
Request rate
Timeouts
Retries
Recursion
Extensions
Target count
```

Do not optimize only for maximum speed.

---

# Scope

A discovered resource may redirect or reference:

```text
CDNs
Cloud providers
SaaS
External authentication
Third-party APIs
```

Wordlist discovery does not expand authorization.

Always perform:

```text
Scope Check
```

before deeper testing.

---

# Recommended Web Workflow

```text
Authorized Target
      ↓
Fingerprint
      ↓
robots.txt / sitemap.xml
      ↓
Katana
      ↓
GAU / Waybackurls
      ↓
Target-Specific List
      ↓
Relevant SecLists List
      ↓
FFUF / Feroxbuster
      ↓
Filter Results
      ↓
Burp / Caido
      ↓
Manual Validation
```

---

# Recommended DNS Workflow

```text
Root Domain
    ↓
Subfinder / Amass
    ↓
Passive Results
    ↓
DNS Validation
    ↓
Relevant SecLists DNS List
    ↓
Scoped Active Enumeration
    ↓
Deduplicate
```

---

# Recommended Password Audit Workflow

```text
Authorized Hash Material
       ↓
Identify Hash Format
       ↓
Small Relevant Password List
       ↓
Rules / Transformations
       ↓
Broader Candidates if Needed
       ↓
Analyze Password Weakness
```

---

# Key Takeaway

Think of SecLists as:

```text
SECURITY TESTING
      |
      +-- Discovery
      +-- Web Content
      +-- DNS
      +-- Usernames
      +-- Passwords
      +-- Fuzzing
      +-- Test Data
```

The most important skill is not knowing where the largest file is.

It is knowing:

```text
WHICH LIST
   +
WHICH TOOL
   +
WHICH TARGET
   +
WHICH TESTING OBJECTIVE
```

belong together.

Use:

```text
Relevant Input
     ↓
Controlled Testing
     ↓
Filtered Results
     ↓
Manual Validation
```

rather than:

```text
Huge List
   +
Maximum Threads
   =
Good Security Testing
```
