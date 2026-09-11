# GAU

> GAU fundamentals for historical URL discovery and authorized web reconnaissance.

## What is GAU?

**GAU = Get All URLs**

GAU is a reconnaissance tool used to collect known/historical URLs associated with a domain from public data sources.

Conceptually:

```text
Domain
  |
  v
 GAU
  |
  +-- Historical URLs
  +-- API endpoints
  +-- Parameters
  +-- Files
  +-- Old paths
```

GAU is particularly useful because the **current website is not the entire historical attack surface**.

---

# Why Historical URLs Matter

Suppose the current application exposes:

```text
/
 /login
 /profile
```

Historical datasets may contain:

```text
/api/v1/users
/admin-old/
 /backup.zip
 /debug
 /download?id=123
```

Some old endpoints may still exist even though they are no longer linked from the current application.

Therefore:

```text
Current Crawler
      +
Historical URLs
      =
Better Coverage
```

---

# Basic Usage

A common usage pattern is:

```bash
gau example.com
```

Use only domains covered by the authorized scope.

Save results:

```bash
gau example.com > gau.txt
```

---

# Deduplicate

Historical sources frequently contain duplicates.

Use:

```bash
sort -u gau.txt -o gau.txt
```

Count:

```bash
wc -l gau.txt
```

---

# Input Pipeline

GAU can also fit into recon pipelines.

Conceptually:

```text
Domains
   ↓
GAU
   ↓
URLs
```

For a list of authorized domains:

```bash
cat domains.txt | gau
```

Save:

```bash
cat domains.txt | gau > urls.txt
```

---

# What Can GAU Discover?

Results may include:

```text
HTML pages
JavaScript
CSS
Images
API endpoints
Query parameters
Documents
Old application paths
Static resources
```

Example:

```text
https://example.com/api/user?id=123
```

This reveals:

```text
Endpoint:
/api/user

Parameter:
id
```

---

# Historical Data

GAU can collect URLs from public historical/indexing sources supported by the installed version.

Conceptually:

```text
Archive / Index Sources
         |
         v
        GAU
         |
         v
Historical URLs
```

Because source support can evolve, use:

```bash
gau --help
```

to inspect current options.

---

# Historical != Currently Live

Critical distinction:

```text
URL found in archive
        !=
URL currently exists
```

A URL may be:

```text
Deleted
Moved
Changed
Expired
Historical only
```

Therefore:

```text
GAU
 ↓
Historical URLs
 ↓
Live Validation
```

---

# GAU + httpx

A useful workflow:

```text
GAU
 ↓
URLs
 ↓
httpx
 ↓
Currently reachable URLs
```

Conceptually:

```bash
cat gau.txt | httpx
```

For large datasets, normalize and scope-check first.

---

# Parameter Discovery

Historical URLs are particularly useful for finding parameters.

Example:

```text
https://example.com/product?id=100
https://example.com/search?q=test
https://example.com/download?file=report.pdf
```

Extracted parameter names:

```text
id
q
file
```

These become useful leads during manual application analysis.

---

# Query Strings

A URL may contain:

```text
/path?parameter=value
```

Example:

```text
/profile?user=alice
```

Breakdown:

```text
Path:
/profile

Parameter:
user

Value:
alice
```

The parameter's presence is not itself a vulnerability.

---

# Interesting Extensions

Historical results can be filtered for potentially useful file types.

Examples:

```text
.js
.json
.xml
.txt
.pdf
.zip
```

For example:

```bash
grep -Ei '\.js($|\?)' gau.txt
```

This can help isolate JavaScript URLs.

---

# JavaScript Discovery

JavaScript often contains:

```text
API routes
Endpoint names
Configuration
Feature flags
Parameter names
```

Workflow:

```text
GAU
 ↓
Historical JS URLs
 ↓
Validate
 ↓
Review JavaScript
 ↓
Additional Endpoints
```

---

# API Discovery

Filter URLs containing likely API paths:

```bash
grep -Ei '/api/' gau.txt
```

Possible output:

```text
/api/v1/users
/api/v2/orders
/api/auth
```

Again:

```text
API Endpoint Found
      !=
Vulnerability
```

---

# Interesting Keywords

Triage can search for terms such as:

```text
admin
api
debug
backup
upload
download
login
auth
internal
dev
test
```

Example:

```bash
grep -Ei 'admin|api|debug|backup' gau.txt
```

Use this only for prioritization.

---

# Sensitive Data in URLs

Historical datasets may contain old query strings.

Occasionally URLs can expose sensitive-looking values.

Examples conceptually:

```text
?token=...
?key=...
?email=...
```

Do not assume such values remain valid.

Also avoid unnecessarily storing or sharing sensitive data discovered during an assessment.

---

# URL Normalization

Historical URL collections can contain many variants:

```text
/page?id=1
/page?id=2
/page?id=3
```

For endpoint analysis, these may represent the same basic route:

```text
/page?id=
```

Normalization reduces noise.

---

# GAU + Waybackurls

Both tools are useful for historical URL discovery.

Conceptually:

```text
GAU -----------+
               |
Waybackurls ---+--> Merge
               |
               v
          Historical URLs
```

Example:

```bash
cat gau.txt wayback.txt | sort -u > historical.txt
```

Different sources/tools may produce different coverage.

---

# GAU + Katana

They complement each other.

```text
GAU
 |
Historical URLs
```

versus:

```text
Katana
 |
Current crawling
```

Combine:

```text
Historical Attack Surface
          +
Current Attack Surface
          ↓
Better Endpoint Coverage
```

---

# GAU + FFUF

Historical data can improve wordlists.

Example:

```text
GAU
 ↓
Known historical paths
 ↓
Extract path components
 ↓
Custom wordlist
 ↓
FFUF
```

Custom wordlists often outperform generic brute-force lists.

---

# GAU + Burp / Caido

Workflow:

```text
GAU
 ↓
Interesting Historical Endpoint
 ↓
Validate Current Availability
 ↓
Burp / Caido
 ↓
Manual Analysis
```

---

# GAU vs Directory Brute Force

GAU:

```text
Known/historical URL discovery
```

FFUF/Gobuster:

```text
Candidate path guessing
```

Therefore:

```text
GAU
=
What has been observed before?
```

while:

```text
FFUF
=
What might exist?
```

Combining both gives better coverage.

---

# Scope Considerations

Historical datasets can contain:

```text
Third-party URLs
CDNs
External authentication providers
Analytics services
Old infrastructure
```

Every discovered URL must still pass:

```text
Scope Validation
```

before active testing.

---

# Recon Workflow

```text
Root Domain
    ↓
Subfinder / Amass
    ↓
Subdomains
    ↓
GAU
    ↓
Historical URLs
    ↓
Deduplicate
    ↓
Filter / Normalize
    ↓
Validate Live URLs
    ↓
Burp / Caido
    ↓
Manual Testing
```

---

# Useful Processing

Unique URLs:

```bash
sort -u gau.txt
```

Find URLs with parameters:

```bash
grep '?' gau.txt
```

Find JavaScript:

```bash
grep -Ei '\.js($|\?)' gau.txt
```

Find APIs:

```bash
grep -Ei '/api/' gau.txt
```

---

# Key Takeaway

GAU answers:

```text
"What URLs associated with this
domain have public data sources
observed?"
```

It does not answer:

```text
"Which URLs are vulnerable?"
```

Use:

```text
Historical Discovery
       ↓
Scope Validation
       ↓
Current Validation
       ↓
Manual Security Analysis
```

GAU is especially valuable for finding **forgotten and unlinked attack surface** that a normal crawler may never encounter.
