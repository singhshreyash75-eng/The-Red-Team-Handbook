# OneListForAll

> OneListForAll fundamentals for web-content discovery and efficient wordlist selection during authorized assessments.

## What is OneListForAll?

**OneListForAll** is a large web-content discovery wordlist designed to combine useful path/file candidates into a practical enumeration dataset.

It can be used with tools such as:

```text
FFUF
Feroxbuster
Gobuster
Dirsearch-style tools
```

Conceptually:

```text
Large Web Path Dataset
        ↓
OneListForAll
        ↓
Content Discovery Tool
        ↓
Hidden / Unlinked Resources
```

---

# Core Purpose

Web applications may contain resources not linked from the visible interface.

Example:

```text
/
 |
 +-- /login
 |
 +-- /assets
 |
 +-- /admin
 |
 +-- /api
 |
 +-- /backup
```

A crawler may find only linked resources.

A wordlist-driven tool asks:

```text
"What else might exist?"
```

---

# Wordlist Discovery

Suppose the list contains:

```text
admin
api
backup
dashboard
internal
uploads
```

A tool can test:

```text
/admin
/api
/backup
/dashboard
/internal
/uploads
```

---

# OneListForAll + FFUF

Typical authorized-lab structure:

```bash
ffuf \
-w onelistforall.txt \
-u https://example.test/FUZZ
```

Conceptually:

```text
OneListForAll
      ↓
FUZZ
      ↓
HTTP Requests
      ↓
Response Analysis
```

---

# OneListForAll + Feroxbuster

```bash
feroxbuster \
-u https://example.test \
-w onelistforall.txt
```

Useful when recursive discovery is desired.

---

# OneListForAll + Gobuster

Conceptually:

```bash
gobuster dir \
-u https://example.test \
-w onelistforall.txt
```

---

# Why Large Combined Lists?

Different applications use different naming patterns.

Examples:

```text
/admin
/administrator
/control
/dashboard
/manage
/management
```

A broad list increases the chance of finding unusual but common real-world paths.

---

# Trade-Off

Large lists increase:

```text
Coverage
```

but also:

```text
Requests
Runtime
Noise
Server load
WAF interaction
```

Therefore:

```text
More Candidates
      !=
Always Better Testing
```

---

# Use Progressive Enumeration

A better methodology:

```text
Small Wordlist
      ↓
Quick Results
      ↓
Target-Specific List
      ↓
Medium/Large List
      ↓
Deep Enumeration if Needed
```

This is more efficient than immediately testing every possible entry.

---

# Fingerprint First

Before enumeration, identify:

```text
Web server
Framework
Language
CMS
API style
Application purpose
```

Example:

```text
IIS / ASP.NET
```

may justify different file-extension candidates than:

```text
PHP Application
```

---

# Extensions

Relevant extensions may include:

```text
.php
.aspx
.jsp
.html
.json
.xml
.txt
```

Do not blindly multiply every wordlist entry by dozens of extensions.

Example:

```text
1,000,000 paths
×
10 extensions
=
10,000,000+ requests
```

per target.

---

# Establish Baseline

Before running a large list:

```text
Request Random Invalid Path
       ↓
Observe Response
```

Record:

```text
Status
Size
Words
Lines
Redirect
```

This helps distinguish valid resources from custom error responses.

---

# Custom 404

Example:

```text
/random-invalid
       ↓
200 OK
       ↓
Same 4500-byte error page
```

If every candidate produces this response:

```text
Thousands of false positives
```

Use appropriate filtering.

---

# 401 and 403

Do not discard automatically:

```text
401 Unauthorized
403 Forbidden
```

They can reveal protected resources.

Example:

```text
/admin -> 403
```

may tell you:

```text
/admin probably exists
```

but not:

```text
/admin is vulnerable
```

---

# Redirects

Useful results can include:

```text
301
302
307
308
```

Example:

```text
/admin
   ↓
301
   ↓
/admin/
```

This can indicate a valid directory.

---

# OneListForAll + Katana

Crawler:

```text
Katana
 ↓
Known/current URLs
```

Wordlist:

```text
OneListForAll
 ↓
Potential hidden URLs
```

Combine:

```text
Current
+
Hidden
=
Broader Coverage
```

---

# OneListForAll + GAU

Historical URLs provide another data source:

```text
GAU
 ↓
Historical Paths
```

Combined with:

```text
OneListForAll
 ↓
General Candidates
```

you get:

```text
Historical
+
Generic
+
Current
```

attack-surface discovery.

---

# OneListForAll + Waybackurls

Same principle:

```text
Waybackurls
      ↓
Old Paths
      +
OneListForAll
      ↓
Candidate Paths
```

---

# Custom Target Wordlist

Target-specific words can be extracted from:

```text
Company terminology
Application routes
JavaScript
API documentation
Historical URLs
Observed filenames
```

Then combine:

```bash
cat target.txt onelistforall.txt |
sort -u > combined.txt
```

---

# Deduplication

Always deduplicate merged lists:

```bash
sort -u combined.txt -o combined.txt
```

This prevents unnecessary duplicate requests.

---

# Recursion

Suppose enumeration discovers:

```text
/api/
```

Further enumeration may find:

```text
/api/v1
/api/docs
/api/admin
```

Tools such as Feroxbuster can automate this recursive process.

Large lists + deep recursion can create enormous traffic, so control depth.

---

# Authenticated Content

Some paths may exist only after authentication.

Where explicitly authorized:

```text
Authorized Session
      ↓
Content Discovery
      ↓
Protected Application Surface
```

Protect:

```text
Cookies
Tokens
Authorization headers
```

as credentials.

---

# Scope Considerations

Content discovery is active testing.

Ensure:

```text
Host
Path
Port
Protocol
```

are within the assessment scope.

Be especially careful with redirects to external services.

---

# Rate Limiting

Large lists can trigger:

```text
429 Too Many Requests
WAF blocks
Temporary bans
Application degradation
```

Use reasonable:

```text
Threads
Rate
Timeouts
Retries
```

---

# Wordlist Strategy

Think:

```text
Quality
   >
Raw Size
```

A smaller list based on:

```text
Technology
Historical data
Current crawler results
Target vocabulary
```

can outperform a huge generic list.

---

# Content Discovery Pipeline

```text
Live Application
      ↓
robots.txt
      ↓
sitemap.xml
      ↓
Katana
      ↓
GAU / Waybackurls
      ↓
Target-Specific Words
      ↓
OneListForAll
      ↓
FFUF / Feroxbuster
      ↓
Manual Validation
```

---

# OneListForAll vs SecLists

```text
OneListForAll
      |
Focused consolidated web-content list
```

```text
SecLists
      |
Large collection of many
different security wordlist categories
```

---

# OneListForAll vs Assetnote

```text
Assetnote
    |
Data-driven wordlists
```

```text
OneListForAll
    |
Consolidated broad web-content list
```

Use whichever better matches the target and traffic budget.

---

# Key Takeaway

OneListForAll is useful for:

```text
WEB CONTENT
     ↓
WORDLIST ENUMERATION
     ↓
HIDDEN / UNLINKED PATHS
```

But do not equate:

```text
More Requests
=
Better Assessment
```

The better strategy is:

```text
Fingerprint
   ↓
Crawl
   ↓
Historical Recon
   ↓
Targeted Wordlist
   ↓
Broader Wordlist
   ↓
Manual Validation
```

Use large lists when they add meaningful coverage—not simply because they are available.
