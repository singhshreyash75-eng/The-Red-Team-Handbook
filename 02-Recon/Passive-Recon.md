# 🕵️ Passive Reconnaissance

> Passive reconnaissance gathers information about a target primarily from existing public sources, without directly probing the target's infrastructure.

---

## Overview

Passive recon is normally the safest place to begin an authorized assessment.

Instead of immediately scanning infrastructure, you first investigate information that already exists publicly.

Examples:

- Domain registration data
- DNS records
- Certificate Transparency logs
- Search engine results
- Public GitHub repositories
- Historical URLs
- Public documents
- Internet-wide datasets

The objective is to build an initial picture of the organization's externally visible attack surface.

---

## Why Passive Recon Matters

Passive recon may reveal:

- Domains
- Subdomains
- IP addresses
- Email patterns
- Technology references
- Cloud services
- Development environments
- Historical applications
- Public code repositories
- Publicly exposed documents

A single discovered hostname such as:

```text
staging.example.com
```

may become significantly more interesting than the main website.

---

## Passive Recon Workflow

```text
Target Domain
      ↓
WHOIS / RDAP
      ↓
DNS Information
      ↓
Certificate Transparency
      ↓
Search Engines
      ↓
Public Code
      ↓
Historical Data
      ↓
Asset List
```

---

## WHOIS and RDAP

Domain registration information can provide useful context.

Traditional command:

```bash
whois example.com
```

Potential information:

```text
Registrar
Creation Date
Expiration Date
Name Servers
Organization
```

Modern registration systems increasingly use **RDAP** as a structured replacement for WHOIS.

### Red Team Value

Look for:

- Related domains
- Naming conventions
- Name servers
- Registrar information

Do not assume registration contact information is current; privacy services are also common.

---

## DNS Information

Basic queries:

```bash
dig example.com
```

Name servers:

```bash
dig NS example.com
```

Mail servers:

```bash
dig MX example.com
```

TXT records:

```bash
dig TXT example.com
```

TXT records may contain:

- SPF configuration
- Domain verification strings
- Third-party service references

---

## Certificate Transparency

Publicly trusted TLS certificates are generally recorded in Certificate Transparency logs.

These records can reveal hostnames such as:

```text
api.example.com
vpn.example.com
staging.example.com
legacy.example.com
```

A commonly used CT search interface is:

```text
crt.sh
```

Certificate data is especially useful because old certificates may expose historical infrastructure.

---

## Search Engine Recon

Search engines can reveal publicly indexed content.

Useful search operators include:

```text
site:
filetype:
intitle:
inurl:
```

Example:

```text
site:example.com
```

Search for documents:

```text
site:example.com filetype:pdf
```

Search for login-related pages:

```text
site:example.com inurl:login
```

Use search operators for discovery, not to bypass authorization boundaries.

---

## Public Documents

Organizations frequently publish:

- PDF files
- Word documents
- Presentations
- Spreadsheets

These documents may expose metadata such as:

```text
Author
Software
Creation Tool
Organization
```

Metadata can sometimes reveal naming conventions or technologies.

---

## GitHub Recon

Public repositories may contain:

- Domain names
- API endpoints
- Configuration examples
- Technology references
- Documentation

Useful searches may include the organization's public name or known domain.

Never treat exposed credentials as permission to access unrelated systems.

GitHub reconnaissance is covered separately in:

```text
GitHub-Recon.md
```

---

## Historical URLs

Applications change over time, but archived URLs may remain visible through public datasets.

Useful tools:

```text
gau
waybackurls
```

Potential discoveries:

```text
/api/v1/
/old-login
/admin-old
/debug
/upload
```

A historical URL being discovered does not guarantee that it still exists.

---

## Passive Subdomain Discovery

A common tool is:

```bash
subfinder -d example.com -silent
```

Save results:

```bash
subfinder -d example.com -silent > passive-subs.txt
```

Subfinder aggregates information from passive sources.

Other tools and sources may return different results.

---

## Public Internet Search Engines

Services such as:

- Shodan
- Censys

index information about Internet-accessible systems.

They may provide:

- Certificates
- Service banners
- Open ports
- Technologies
- Historical observations

Always verify that any discovered system falls within the authorized scope before interacting with it.

---

## Email Pattern Discovery

Public pages may reveal corporate email patterns.

Example:

```text
firstname.lastname@example.com
```

This can help understand organizational naming conventions.

It should not be used as justification for credential attacks unless explicitly authorized.

---

## Passive Recon Notes

Keep results structured.

Example:

```text
Target:
example.com

Name Servers:
ns1.provider.com
ns2.provider.com

Mail:
mail.example.com

Subdomains:
api.example.com
dev.example.com
vpn.example.com

Interesting:
dev.example.com
vpn.example.com
```

---

## Useful Tools

| Tool / Source | Purpose |
|---|---|
| WHOIS/RDAP | Registration data |
| `dig` | DNS information |
| crt.sh | Certificate Transparency |
| Subfinder | Passive subdomains |
| Amass | Asset discovery |
| Shodan | Internet exposure data |
| Censys | Internet/certificate datasets |
| gau | Historical URLs |
| waybackurls | Archived URLs |

---

## Red Team Perspective

Passive recon is useful for answering:

> What does the Internet already know about this organization?

The strongest findings often come from correlating information.

Example:

```text
Certificate
    ↓
staging.example.com
    ↓
Public Repository
    ↓
References /api/v2
    ↓
In-Scope Application
```

Correlation matters more than running dozens of tools.

---

## Common Beginner Mistakes

- Assuming passive results are current.
- Treating every discovered asset as in scope.
- Collecting thousands of results without reviewing them.
- Ignoring Certificate Transparency.
- Ignoring historical information.
- Relying on only one data source.

---

## Interview Questions

### What is passive reconnaissance?

Gathering information primarily from existing sources without directly probing the target infrastructure.

### Why use Certificate Transparency?

Certificates can reveal current and historical hostnames.

### WHOIS vs RDAP?

Both provide registration information; RDAP provides a more modern structured protocol/API model.

### Why combine multiple passive sources?

Each source has different coverage and historical data.

---

## Quick Revision

```text
WHOIS/RDAP → Registration
DNS → Infrastructure clues
CT Logs → Subdomains
Search Engines → Indexed content
GitHub → Public code/references
Archives → Historical endpoints
Shodan/Censys → Existing Internet observations
```

---

## References

- ICANN RDAP Documentation
- Certificate Transparency Project
- ProjectDiscovery Subfinder Documentation
- OWASP Web Security Testing Guide
- Shodan Documentation
- Censys Documentation
