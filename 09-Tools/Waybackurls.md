# Waybackurls

> Historical URL discovery using web archives for authorized reconnaissance and attack-surface mapping.

## What is Waybackurls?

**Waybackurls** is a reconnaissance tool used to retrieve historical URLs associated with domains.

It primarily helps discover URLs that have previously been observed by web-archive/indexing sources.

Conceptually:

```text
Domain
   ↓
Historical Web Data
   ↓
Waybackurls
   ↓
Old / Known URLs
```

It is useful for finding:

```text
Old endpoints
API routes
Parameters
JavaScript files
Documents
Legacy pages
Unlinked resources
```

---

# Why Historical URLs Matter

The current application may expose:

```text
/
 /login
 /profile
```

but historical data may contain:

```text
/admin-old/
/api/v1/users
/debug/
/backup/
/download?id=123
```

Some endpoints may still exist even though the current website no longer links to them.

Therefore:

```text
Current Application
       +
Historical Attack Surface
       =
Better Coverage
```

---

# Basic Usage

For an authorized domain:

```bash
echo example.com | waybackurls
```

Save results:

```bash
echo example.com | waybackurls > wayback.txt
```

For a domain list:

```bash
cat domains.txt | waybackurls
```

Check installed-version behavior where necessary.

---

# Deduplicate Results

Historical data often contains duplicates.

```bash
sort -u wayback.txt -o wayback.txt
```

Count:

```bash
wc -l wayback.txt
```

Conceptually:

```text
Raw URLs
   ↓
Deduplicate
   ↓
Cleaner Dataset
```

---

# Historical != Live

Critical distinction:

```text
URL in Archive
      !=
Currently Available URL
```

Historical URLs may now be:

```text
Deleted
Moved
Offline
Changed
Out of scope
```

Therefore:

```text
Waybackurls
     ↓
Historical URLs
     ↓
Current Validation
```

---

# URL Anatomy

Example:

```text
https://example.com/product?id=100
```

Breakdown:

```text
Scheme:
https

Host:
example.com

Path:
/product

Parameter:
id

Value:
100
```

Historical URLs can therefore reveal both:

```text
Endpoints
+
Parameters
```

---

# Parameter Discovery

One of the most useful outputs is URLs containing query parameters.

Example:

```text
/search?q=test
/product?id=123
/download?file=report.pdf
/profile?user=alice
```

Extract URLs containing `?`:

```bash
grep '?' wayback.txt
```

Possible parameter names:

```text
q
id
file
user
```

These become leads for manual testing.

---

# Parameter Found != Vulnerability

Finding:

```text
/download?file=
```

does not automatically mean:

```text
Path traversal
```

Likewise:

```text
/product?id=
```

does not automatically mean:

```text
IDOR
```

The correct workflow is:

```text
Parameter Discovery
        ↓
Current Validation
        ↓
Manual Security Testing
```

---

# JavaScript Discovery

Historical data can reveal old JavaScript files.

Filter:

```bash
grep -Ei '\.js($|\?)' wayback.txt
```

JavaScript may contain references to:

```text
API endpoints
Routes
Parameter names
Application functionality
Configuration
Feature names
```

---

# JavaScript Example

Suppose historical JavaScript contains:

```javascript
fetch("/api/v1/account")
```

This reveals:

```text
/api/v1/account
```

even if the current UI does not link to it.

---

# API Discovery

Filter likely API routes:

```bash
grep -Ei '/api/' wayback.txt
```

Possible results:

```text
/api/v1/users
/api/v1/orders
/api/auth
/api/upload
```

Then determine whether those routes still exist.

---

# Interesting File Types

Historical data may expose references to:

```text
.json
.xml
.txt
.pdf
.zip
.js
```

Filtering can help prioritize investigation.

Example:

```bash
grep -Ei '\.(json|xml|txt|zip)($|\?)' wayback.txt
```

Do not access sensitive files beyond assessment authorization.

---

# Interesting Keywords

Useful triage terms:

```text
admin
api
auth
debug
backup
upload
download
internal
dev
test
old
config
```

Example:

```bash
grep -Ei 'admin|api|debug|backup|upload' wayback.txt
```

These are only leads.

---

# Waybackurls + httpx

A useful workflow:

```text
Waybackurls
      ↓
Historical URLs
      ↓
httpx
      ↓
Currently Reachable URLs
```

Conceptually:

```bash
cat wayback.txt | httpx
```

For large datasets, normalize and scope-check before active probing.

---

# Waybackurls + Katana

These tools provide complementary views.

```text
Waybackurls
     |
Historical URLs
```

versus:

```text
Katana
     |
Current Crawling
```

Combine:

```text
Historical
    +
Current
    ↓
Broader Endpoint Dataset
```

---

# Waybackurls + GAU

Both focus on historical/known URL discovery.

Conceptually:

```text
Waybackurls ---+
               |
               +--> Merge --> sort -u
               |
GAU -----------+
```

Example:

```bash
cat wayback.txt gau.txt |
sort -u > historical-urls.txt
```

Different tools/data sources may provide different coverage.

---

# Waybackurls + FFUF

Historical paths can improve custom wordlists.

Conceptually:

```text
Waybackurls
     ↓
Historical Paths
     ↓
Extract Useful Words
     ↓
Custom Wordlist
     ↓
FFUF
```

This can be more targeted than blindly using huge generic lists.

---

# Waybackurls + Burp / Caido

Workflow:

```text
Historical URL
      ↓
Confirm Currently Reachable
      ↓
Burp / Caido
      ↓
Manual Request Analysis
```

This is especially useful for old APIs and parameters.

---

# Historical Parameters

Suppose archives contain:

```text
/view?id=10
/view?id=11
/view?id=12
```

Instead of treating these as three separate endpoints, recognize:

```text
/view?id=
```

as the underlying parameterized route.

Normalization reduces noise.

---

# Sensitive Data in Archived URLs

Historical URLs may contain old values resembling:

```text
Tokens
Email addresses
File names
Identifiers
API parameters
```

Do not assume:

```text
Historical Secret
=
Currently Valid Secret
```

Still treat potentially sensitive data carefully.

---

# Archived Responses

Waybackurls primarily helps identify URLs.

If historical content itself is needed, web-archive systems may separately provide archived responses depending on availability.

Do not confuse:

```text
URL Discovery
```

with:

```text
Archived Content Retrieval
```

---

# Scope Considerations

Historical URLs can reference:

```text
CDNs
Third-party APIs
Analytics
SSO providers
SaaS services
Old domains
External storage
```

Therefore:

```text
URL Discovered
      ↓
Scope Validation
      ↓
Authorized?
```

Only then should active testing proceed.

---

# Third-Party Infrastructure

Example:

```text
example.com
   ↓
Historical redirect
   ↓
thirdparty.example
```

The third-party service is not automatically authorized simply because it appeared in historical data.

---

# Recon Pipeline

A strong workflow:

```text
Root Domain
    ↓
Subfinder / Amass
    ↓
Subdomains
    ↓
httpx
    ↓
Live Applications
    |
    +----------------+
    |                |
  Katana       GAU / Waybackurls
    |                |
    +----------------+
             |
          Merge URLs
             |
          Deduplicate
             |
          Scope Check
             |
       Current Validation
             |
        Burp / Caido
```

---

# Current + Historical + Hidden

Three complementary approaches:

```text
Katana
=
Current linked/crawlable URLs
```

```text
Waybackurls / GAU
=
Historical URLs
```

```text
FFUF / Feroxbuster
=
Potential hidden content
```

Together:

```text
Current
+
Historical
+
Guessed
=
Broader Web Attack Surface
```

---

# False Positives / Noise

Historical datasets commonly contain:

```text
Duplicate URLs
Dead links
Tracking parameters
Old applications
Third-party resources
Static assets
Obsolete endpoints
```

Therefore always:

```text
Collect
 ↓
Normalize
 ↓
Deduplicate
 ↓
Filter
 ↓
Validate
```

---

# Useful Quick Commands

Collect:

```bash
echo example.com | waybackurls > wayback.txt
```

Deduplicate:

```bash
sort -u wayback.txt -o wayback.txt
```

Parameters:

```bash
grep '?' wayback.txt
```

JavaScript:

```bash
grep -Ei '\.js($|\?)' wayback.txt
```

API:

```bash
grep -Ei '/api/' wayback.txt
```

Interesting paths:

```bash
grep -Ei 'admin|api|debug|backup|upload' wayback.txt
```

---

# Key Takeaway

Waybackurls answers:

```text
"What URLs associated with this
domain have appeared historically?"
```

It does **not** answer:

```text
"Which URLs are currently vulnerable?"
```

Use:

```text
HISTORICAL DISCOVERY
        ↓
NORMALIZE
        ↓
SCOPE CHECK
        ↓
CURRENT VALIDATION
        ↓
MANUAL ANALYSIS
```

Historical reconnaissance is valuable because:

```text
Removed from UI
      !=
Removed from Server
```

and forgotten endpoints can remain part of the attack surface.
