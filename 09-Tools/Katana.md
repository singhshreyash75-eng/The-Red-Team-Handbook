# Katana

> ProjectDiscovery Katana fundamentals for authorized web crawling, endpoint discovery, and attack-surface mapping.

## What is Katana?

**Katana** is a web crawler developed by ProjectDiscovery.

It is designed to discover application resources and endpoints by crawling web content.

Conceptually:

```text
Website
   ↓
Katana
   ↓
Links / Resources
   ↓
Endpoints
   ↓
Expanded Attack Surface
```

---

# Crawler vs Fuzzer

Important distinction:

```text
Katana
=
Crawls links/resources that
applications expose
```

while:

```text
FFUF / Gobuster
=
Guess candidate paths
from wordlists
```

Conceptually:

```text
Crawler
   ↓
Follow what exists/is referenced
```

versus:

```text
Fuzzer
   ↓
Guess what might exist
```

Both approaches complement each other.

---

# Basic Usage

A common authorized-lab pattern:

```bash
katana -u https://example.test
```

Check installed-version options:

```bash
katana -h
```

---

# Input List

For multiple authorized targets:

```bash
katana -list live.txt
```

Conceptually:

```text
live.txt
   ↓
Katana
   ↓
URLs / Endpoints
```

Exact flags may vary by version.

---

# What Katana Can Discover

Crawling can identify:

```text
Pages
API endpoints
JavaScript files
Forms
Parameters
Static assets
Linked resources
Application routes
```

Example:

```text
/
 |
 +-- /login
 +-- /dashboard
 +-- /api/users
 +-- /static/app.js
```

---

# Crawling Depth

Applications contain nested links.

Conceptually:

```text
Depth 0
/

Depth 1
/login
/products

Depth 2
/products/1
/products/2

Depth 3
/api/products/1
```

Increasing crawl depth may discover more content but also generates more traffic.

---

# Current vs Historical Discovery

Katana primarily explores the currently reachable application.

Compare:

```text
Katana
 |
Current application crawling
```

with:

```text
GAU / Waybackurls
 |
Historical URL discovery
```

A strong recon methodology combines both.

---

# Katana + GAU

```text
Katana
   |
Current URLs
   |
   +------+
          |
          v
       Merge
          ^
          |
   +------+
   |
GAU
 |
Historical URLs
```

Result:

```text
Current
+
Historical
=
Broader Endpoint Coverage
```

---

# JavaScript Discovery

Modern applications often place significant functionality in JavaScript.

Katana may discover:

```text
/app.js
/main.js
/chunk.js
```

JavaScript can reference:

```text
API routes
Internal endpoints
Parameters
Feature names
Application paths
```

---

# JavaScript Endpoint Concept

Example JavaScript:

```javascript
fetch("/api/v1/profile")
```

This reveals:

```text
/api/v1/profile
```

even if no visible HTML link points to it.

---

# Forms

Crawlers can help identify application forms.

Example:

```html
<form action="/login" method="POST">
```

This reveals:

```text
Endpoint:
/login

Method:
POST
```

Forms are useful for mapping application functionality.

---

# Parameters

URLs may expose parameters:

```text
/product?id=100
/search?q=test
/profile?user=alice
```

Katana can therefore contribute to parameter discovery.

Remember:

```text
Parameter Found
      !=
Vulnerability
```

---

# Headless Crawling

Modern applications may build routes dynamically using JavaScript.

A simple HTTP crawler may miss them.

Headless browser-based crawling can execute JavaScript and observe dynamically generated navigation.

Conceptually:

```text
HTML
 ↓
JavaScript Executes
 ↓
Dynamic Routes
 ↓
Crawler Discovers More Content
```

Headless crawling is more resource-intensive.

---

# Single Page Applications

SPAs commonly use:

```text
React
Vue
Angular
```

Navigation may occur without traditional full-page links.

Therefore:

```text
JavaScript-aware crawling
```

can improve coverage.

---

# Scope

A crawler can easily follow links to:

```text
CDNs
Analytics
Third-party APIs
SSO providers
External sites
```

Therefore scope control is essential.

Conceptually:

```text
Discovered Link
      ↓
In Scope?
   /       \
 Yes       No
  |         |
Crawl      Stop
```

---

# Query Parameters

Crawlers may encounter many variants:

```text
/product?id=1
/product?id=2
/product?id=3
```

Normalization/deduplication helps reduce noise.

The important endpoint may simply be:

```text
/product?id=
```

---

# Katana + httpx

Typical recon pipeline:

```text
Subfinder / Amass
       ↓
Subdomains
       ↓
httpx
       ↓
Live Applications
       ↓
Katana
       ↓
Endpoints
```

---

# Katana + Nuclei

Conceptually:

```text
Katana
 ↓
Discovered URLs
 ↓
Nuclei
 ↓
Template-Based Checks
```

Apply only appropriate scoped templates and manually validate findings.

---

# Katana + Burp / Caido

A strong workflow:

```text
Katana
 ↓
Interesting Endpoint
 ↓
Burp / Caido
 ↓
Manual Request Analysis
```

Crawling provides coverage.

The proxy provides depth.

---

# Katana + FFUF

Katana finds referenced resources:

```text
Katana
 ↓
Known Application Paths
```

FFUF can test additional candidates:

```text
FFUF
 ↓
Potential Hidden Paths
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

# Output Processing

Save findings:

```bash
katana -u https://example.test > katana.txt
```

Deduplicate:

```bash
sort -u katana.txt -o katana.txt
```

Find URLs containing parameters:

```bash
grep '?' katana.txt
```

Find JavaScript:

```bash
grep -Ei '\.js($|\?)' katana.txt
```

---

# Interesting Paths

Triage terms may include:

```text
api
admin
auth
login
upload
download
debug
internal
graphql
swagger
```

Example:

```bash
grep -Ei 'api|admin|auth|upload|graphql' katana.txt
```

These are prioritization clues only.

---

# robots.txt

Before crawling, inspect:

```text
/robots.txt
```

It may reveal application paths.

But:

```text
robots.txt != Access Control
```

---

# sitemap.xml

Also inspect:

```text
/sitemap.xml
```

A sitemap can provide high-quality URL discovery without guessing.

---

# Recon Strategy

Use several complementary techniques:

```text
Subdomain Discovery
        ↓
Live Host Validation
        ↓
Current Crawling
        +
Historical URLs
        +
Content Discovery
        ↓
Endpoint Dataset
        ↓
Manual Analysis
```

---

# False Positives / Noise

Crawlers may collect:

```text
Logout URLs
Tracking URLs
Duplicate query strings
Static assets
Third-party links
Dead routes
```

Normalize and prioritize before testing.

---

# Operational Considerations

Deep crawling can generate many requests.

Control:

```text
Depth
Concurrency
Rate
Scope
```

Do not blindly crawl enormous applications at maximum speed.

---

# Key Takeaway

Katana answers:

```text
"What endpoints and resources
does this application expose
through crawlable behavior?"
```

Its place in the toolchain is:

```text
LIVE APPLICATION
      ↓
KATANA
      ↓
CURRENT ENDPOINTS
      ↓
MANUAL / AUTOMATED ANALYSIS
```

Combine:

```text
Katana
+
GAU / Waybackurls
+
FFUF / Feroxbuster
```

to cover:

```text
Current
+
Historical
+
Hidden/Guessed
```

web attack surface.
