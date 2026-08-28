# 📁 Content Discovery

> Content discovery is the process of identifying hidden or unlinked files, directories, endpoints, and functionality exposed by a web application.

---

## Overview

A website's visible navigation rarely represents its complete attack surface.

For example, the homepage may expose:

```text
/
/login
/about
/contact
```

while additional content may exist at:

```text
/admin/
/api/
/backup/
/uploads/
/old/
/debug/
/docs/
```

Finding this content can expose functionality that deserves deeper security testing.

---

## Why Content Discovery Matters

Interesting discoveries can include:

- Admin panels
- API endpoints
- Backup files
- Configuration files
- Upload directories
- Development endpoints
- Old application versions
- Documentation
- Debug functionality
- Forgotten resources

Content discovery is therefore a bridge between **Recon** and **Web Pentesting**.

---

## Crawling vs Brute-Force Discovery

These techniques are related but different.

### Crawling

Follow resources already referenced by the application.

```text
Homepage
   ↓
Links
   ↓
JavaScript
   ↓
Forms
   ↓
Endpoints
```

Examples of crawlers:

- Katana
- Burp Spider/Crawler
- ZAP Spider

### Wordlist-Based Discovery

Try candidate paths that may not be linked anywhere.

```text
/admin
/backup
/api
/test
```

Common tools:

- ffuf
- Feroxbuster
- Gobuster

Using both approaches provides better coverage.

---

## Start Manually

Before fuzzing anything, inspect:

```text
robots.txt
sitemap.xml
```

Example:

```bash
curl https://example.com/robots.txt
```

Possible output:

```text
User-agent: *
Disallow: /admin/
Disallow: /backup/
```

`robots.txt` is not an access-control mechanism.

It may actually reveal interesting paths.

---

## sitemap.xml

Check:

```bash
curl https://example.com/sitemap.xml
```

A sitemap may expose:

- Pages
- Blog routes
- Product URLs
- Legacy endpoints

---

## FFUF

FFUF is a fast web fuzzer frequently used for content discovery.

Basic authorized/lab example:

```bash
ffuf -w wordlist.txt -u https://example.com/FUZZ
```

`FUZZ` represents the location where wordlist entries are inserted.

Conceptually:

```text
Wordlist

admin
api
backup

        ↓

/admin
/api
/backup
```

---

## File Extension Discovery

Applications may expose files such as:

```text
.php
.txt
.json
.xml
.bak
```

When using fuzzing tools, extension testing should be based on the technology you have already identified.

For example, testing PHP-related resources makes more sense against a PHP application than blindly trying every possible extension.

---

## Feroxbuster

Feroxbuster performs recursive content discovery.

Typical lab-style usage:

```bash
feroxbuster -u https://example.com -w wordlist.txt
```

Recursive discovery can identify nested paths such as:

```text
/admin/
/admin/users/
/admin/config/
```

---

## Gobuster

Another common directory discovery tool.

Example:

```bash
gobuster dir -u https://example.com -w wordlist.txt
```

---

## Useful Wordlists

SecLists contains commonly used web-content wordlists.

Typical location on Kali:

```text
/usr/share/seclists/Discovery/Web-Content/
```

Examples include:

```text
common.txt
directory-list-2.3-medium.txt
raft-small-words.txt
```

Choose wordlists intelligently rather than always using the largest available list.

---

## HTTP Status Codes

Don't look only for `200`.

| Status | Meaning during recon |
|---:|---|
| 200 | Accessible |
| 204 | Valid endpoint, no body |
| 301/302 | Redirect |
| 401 | Authentication required |
| 403 | Exists but access restricted |
| 405 | Endpoint exists; method rejected |
| 429 | Rate limited |
| 500 | Application error |

A response such as:

```text
403 /admin
```

can be more interesting than:

```text
404 /admin
```

because `403` indicates that something may exist there.

---

## Response Size

Applications sometimes return:

```text
200 OK
```

for nonexistent paths.

Example:

```text
/random123 → 200 → 4,521 bytes
/admin     → 200 → 8,912 bytes
```

Comparing response sizes can help separate genuine results from custom error pages.

---

## Baseline the Application

Before running discovery, request a random path:

```text
/this-should-not-exist-827361
```

Observe:

- Status code
- Response size
- Redirect behavior
- Page title

This establishes a baseline for nonexistent resources.

---

## Crawling with Katana

Example:

```bash
katana -u https://example.com
```

A crawler may discover:

```text
/login
/api/users
/assets/app.js
/password-reset
```

Crawling is particularly useful for modern JavaScript-heavy applications.

---

## Historical Content

Archived sources can expose old endpoints.

Examples:

```text
gau
waybackurls
```

Potential discoveries:

```text
/api/v1/
/old-admin/
/legacy-login
```

Historical endpoints should be validated carefully because many will no longer exist.

---

## Interesting Names

Pay attention to:

```text
admin
api
backup
config
dashboard
debug
dev
docs
internal
legacy
old
private
staging
test
upload
```

These names don't imply vulnerabilities; they simply help prioritize investigation.

---

## Backup Files

Misconfigured deployments occasionally expose files such as:

```text
config.php.bak
index.php.old
database.sql
backup.zip
```

Backups can accidentally contain source code or configuration information.

---

## Source Maps

Modern JavaScript applications may expose:

```text
app.js.map
main.js.map
```

Source maps can make minified client-side code easier to understand and may reveal original file structures.

They should be treated as an information source, not automatically as a vulnerability.

---

## Burp Suite Workflow

Burp can complement CLI discovery.

Useful features:

```text
Proxy
Target Sitemap
Repeater
Crawler
```

Workflow:

```text
Browse Application
       ↓
Burp Builds Sitemap
       ↓
Review Paths
       ↓
Compare with CLI Discovery
```

---

## Recommended Workflow

```text
Homepage
   ↓
robots.txt / sitemap.xml
   ↓
Manual Browsing
   ↓
Crawling
   ↓
Historical URLs
   ↓
Controlled Wordlist Discovery
   ↓
Review Status + Size
   ↓
Prioritize Interesting Content
```

---

## Red Team Perspective

The objective isn't:

> Find the maximum number of directories.

The objective is:

> Find functionality that changes the security assessment.

For example:

```text
/assets/logo.png
```

is usually less interesting than:

```text
/admin
/api
/upload
/debug
```

Prioritize based on functionality.

---

## Common Beginner Mistakes

- Using enormous wordlists immediately.
- Ignoring `401`, `403`, and redirects.
- Not establishing a 404 baseline.
- Treating every `200` response as valid.
- Running excessive request rates.
- Ignoring `robots.txt`.
- Ignoring JavaScript-discovered endpoints.
- Not verifying results manually.

---

## Interview Questions

### What is content discovery?

Finding hidden or unlinked application resources and functionality.

### Crawling vs directory fuzzing?

Crawling follows known references; fuzzing tests candidate paths.

### Why is a 403 interesting?

It can indicate that a resource exists but is access-controlled.

### Why establish a baseline?

To recognize custom error pages and false positives.

---

## Quick Revision

```text
robots.txt
sitemap.xml
     ↓
Crawl
     ↓
Historical URLs
     ↓
FFUF / Feroxbuster / Gobuster
     ↓
Status + Size Analysis
     ↓
Manual Verification

Interesting:
admin
api
backup
debug
upload
staging
```

---

## References

- OWASP Web Security Testing Guide
- FFUF Documentation
- Feroxbuster Documentation
- Gobuster Documentation
- SecLists
- ProjectDiscovery Katana Documentation
