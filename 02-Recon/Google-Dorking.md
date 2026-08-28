# 🔎 Google Dorking

> Google Dorking, also called Google Hacking, uses advanced search operators to locate specific publicly indexed information more efficiently.

---

## Overview

Normal search:

```text
example company
```

Advanced search:

```text
site:example.com filetype:pdf
```

Search operators allow reconnaissance to focus on:

- Specific domains
- File types
- URL patterns
- Page titles
- Public documents

Google Dorking does **not** magically access private content.

It searches information already indexed by the search engine.

---

## Why It Matters

Search engines can reveal:

- Login portals
- Public documents
- Old pages
- Documentation
- Subdomains
- Public files
- Application paths

It is especially useful during passive reconnaissance.

---

## `site:`

Restrict results to a domain.

```text
site:example.com
```

Possible results:

```text
www.example.com
support.example.com
docs.example.com
```

---

## Excluding the Main Site

Example:

```text
site:example.com -www
```

This can make alternate indexed hostnames easier to notice.

Results depend on the search engine's indexing behavior.

---

## `filetype:`

Search for particular file types.

```text
site:example.com filetype:pdf
```

Other examples:

```text
filetype:docx
filetype:xlsx
filetype:pptx
filetype:txt
```

Public documents can help understand terminology and organizational structure.

---

## `inurl:`

Search for text inside URLs.

Example:

```text
site:example.com inurl:login
```

Other useful concepts:

```text
inurl:admin
inurl:api
inurl:docs
```

---

## `intitle:`

Search page titles.

Example:

```text
site:example.com intitle:"login"
```

This can help locate:

- Portals
- Documentation
- Dashboards

---

## Exact Phrase

Use quotes:

```text
"example.com"
```

This searches for the exact phrase.

Useful for finding references to a domain on other public websites.

---

## Combining Operators

Example:

```text
site:example.com filetype:pdf
```

or:

```text
site:example.com inurl:login
```

or:

```text
site:example.com intitle:"documentation"
```

Combining operators reduces noise.

---

## Public Documents

Example:

```text
site:example.com filetype:pdf
```

Documents may expose:

- Product names
- Infrastructure terminology
- Public employee/contact information
- Software references

Do not assume document metadata is accurate or current.

---

## Login Portals

Example:

```text
site:example.com inurl:login
```

Potential results:

```text
Customer Portal
Employee Portal
Support Login
```

Discovery of a login page does not imply permission for credential attacks.

---

## API Documentation

Search conceptually for:

```text
site:example.com "API"
```

or:

```text
site:example.com inurl:docs
```

API documentation can reveal:

- Endpoint structures
- Authentication models
- Versions

---

## Error Messages

Publicly indexed error pages can sometimes reveal technology information.

For example, searches involving known target terminology plus framework-specific error wording may surface historical pages.

Avoid treating search-engine snippets as authoritative evidence; validate relevant findings within scope.

---

## Other Search Engines

Recon should not depend on one search provider.

Alternatives include:

- Bing
- DuckDuckGo
- Brave Search

Different indexes may produce different results.

---

## Search Engines vs Internet Scanners

Don't confuse:

```text
Google
```

with:

```text
Shodan / Censys
```

Google primarily indexes web content.

Internet-wide scanners index information about network services and certificates.

Both can contribute to passive recon, but their datasets are different.

---

## Search Workflow

```text
Root Domain
    ↓
site:
    ↓
File Types
    ↓
URL Patterns
    ↓
Titles
    ↓
Exact Phrases
    ↓
Document Findings
    ↓
Validate In Scope
```

---

## Record Useful Findings

Example:

```text
Query:
site:example.com inurl:docs

Finding:
docs.example.com

Purpose:
API documentation

Status:
Needs scope validation
```

Recording the source makes recon reproducible.

---

## Red Team Perspective

The value of search operators is not memorizing hundreds of "dorks."

It's knowing how to ask:

```text
What public information about this target
has already been indexed?
```

Build searches based on what you're trying to discover.

---

## Common Beginner Mistakes

- Copying giant "Google Dork lists."
- Searching for data unrelated to the authorized target.
- Treating indexed content as current.
- Assuming every result is in scope.
- Using only Google.
- Failing to document the query that produced a finding.

---

## Interview Questions

### What is Google Dorking?

Using advanced search operators to find specific indexed information.

### What does `site:` do?

Restricts results to a particular domain/site.

### What does `filetype:` do?

Filters results by file type.

### Is Google Dorking exploitation?

No. It is generally information discovery against indexed content.

---

## Quick Revision

```text
site:       → Domain
filetype:   → File Type
inurl:      → URL
intitle:    → Title
"..."       → Exact Phrase

Goal:
Find useful PUBLIC information efficiently.
```

---

## Practice

Use search operators against:

- Your own website
- Purpose-built OSINT exercises
- Authorized targets

Avoid collecting unnecessary personal or sensitive information.

---

## References

- Google Search Help
- OWASP Web Security Testing Guide
- Google Hacking Database (for studying search patterns)
