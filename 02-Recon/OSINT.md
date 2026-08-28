# 🛰️ OSINT — Open Source Intelligence

> Open Source Intelligence (OSINT) is intelligence derived from publicly available information. In Red Teaming, OSINT helps understand an organization's externally visible technology, infrastructure, and operational footprint before deeper testing begins.

---

## Overview

OSINT is broader than simply:

```text
Google Search
```

Potential sources include:

```text
Search Engines
Public Websites
DNS
Certificate Transparency
Public Code
Public Documents
Internet Archives
Public Technical Records
Internet Exposure Databases
```

The objective is to transform publicly available information into useful, relevant intelligence.

---

## OSINT vs Recon

The terms overlap heavily.

A useful distinction:

### OSINT

Focuses on collecting and analyzing publicly available information.

### Reconnaissance

Broader security-assessment process that may include both passive and active techniques.

Conceptually:

```text
OSINT
   ↓
Passive Recon
   ↓
Active Recon
   ↓
Enumeration
```

---

## Intelligence Cycle

Good OSINT isn't random searching.

Use:

```text
Requirements
     ↓
Collection
     ↓
Processing
     ↓
Analysis
     ↓
Reporting
```

---

## 1. Define Intelligence Requirements

Start with questions.

For example:

```text
What domains belong to the organization?

What public technologies are visible?

What cloud providers appear to be used?

What development resources are public?

What externally exposed applications exist?
```

This prevents aimless collection.

---

## 2. Domain Intelligence

Useful sources:

- RDAP / WHOIS
- DNS
- Certificate Transparency
- Search engines

Possible information:

```text
Registrar
Name Servers
Mail Providers
Subdomains
Related infrastructure
```

---

## 3. DNS Intelligence

Queries:

```bash
dig NS example.com
```

```bash
dig MX example.com
```

```bash
dig TXT example.com
```

This can reveal:

- DNS providers
- Email infrastructure
- Third-party services

---

## 4. Certificate Intelligence

Certificate Transparency can reveal:

```text
api.example.com
vpn.example.com
mail.example.com
staging.example.com
```

Historical certificates can also reveal older naming patterns.

---

## 5. Public Web Presence

Review:

```text
Main Website
Documentation
Support Portals
Developer Portals
Status Pages
Career Pages
Public Blogs
```

These sources often reveal technology choices and application names.

---

## 6. Public Code

GitHub and other public code-hosting platforms may reveal:

- Technology stacks
- Public SDKs
- API documentation
- Infrastructure naming
- Open-source components

See:

```text
GitHub-Recon.md
```

for deeper coverage.

---

## 7. Historical Information

Internet archives can provide snapshots of older web applications.

Potential discoveries:

```text
Old URLs
Previous Products
Deprecated APIs
Historical Subdomains
```

Historical information should always be validated before being treated as current.

---

## 8. Internet Exposure Databases

Platforms such as:

```text
Shodan
Censys
```

collect existing observations about Internet-facing services.

Potential information:

- Service banners
- TLS certificates
- Open ports
- Technologies

These platforms are useful because querying their existing datasets can reduce the need for direct scanning during initial reconnaissance.

---

## 9. Public Documents

Organizations publish:

```text
PDF
DOCX
PPTX
XLSX
```

Documents can reveal:

- Technology terminology
- Product names
- Public contact information
- Organizational naming conventions

Metadata should be treated as a clue rather than definitive evidence.

---

## 10. Technology Intelligence

Useful sources include:

- Public websites
- HTTP headers
- Documentation
- Job advertisements
- Public repositories

For example, public technical job descriptions may mention:

```text
AWS
Kubernetes
Python
React
Azure
Splunk
```

This can help understand likely technology usage.

It should not be treated as proof that a specific exposed system uses that technology.

---

## 11. Email Infrastructure

MX records can identify mail providers.

Example:

```bash
dig MX example.com
```

Potential providers:

```text
Microsoft 365
Google Workspace
Custom Mail Infrastructure
```

TXT records can also reveal:

```text
SPF
DKIM-related configuration
Domain verification
```

---

## 12. Cloud Footprint

Public information may reference services from:

```text
AWS
Azure
GCP
Cloudflare
GitHub
Firebase
```

Examples:

```text
Cloud-hosted domains
Public documentation
Repository configuration
DNS records
```

Do not enumerate unrelated cloud resources outside the defined scope.

---

## 13. Third-Party Relationships

Organizations depend on:

- SaaS providers
- CDNs
- Payment processors
- Authentication providers
- Support platforms

These relationships are useful for understanding architecture.

However:

**Third-party infrastructure is not automatically in scope.**

---

## 14. Correlation

This is where OSINT becomes intelligence.

Individual finding:

```text
Public repository references api-stage.example.com
```

Another finding:

```text
Certificate Transparency contains api-stage.example.com
```

Another:

```text
DNS currently resolves it
```

Together:

```text
Public Code
    +
Certificate
    +
DNS
    ↓
High-confidence asset
```

Correlation increases confidence.

---

## OSINT Notes Template

```text
Finding:
api-stage.example.com

Sources:
GitHub
Certificate Transparency

Current DNS:
Resolvable

Relationship:
Target-owned hostname

Scope:
Needs verification

Confidence:
High
```

---

## Confidence Levels

Useful model:

### High Confidence

Multiple independent sources agree.

### Medium Confidence

One strong source.

### Low Confidence

Indirect or historical reference.

This prevents weak assumptions from becoming "facts."

---

## Data Minimization

OSINT can easily become invasive.

Collect only information relevant to the authorized security objective.

Avoid unnecessary collection of:

- Personal addresses
- Family information
- Private personal profiles
- Unrelated personal data

Professional Red Team OSINT should be **objective-driven**, not voyeuristic.

---

## Useful OSINT Tools / Sources

| Tool / Source | Purpose |
|---|---|
| RDAP / WHOIS | Domain registration |
| `dig` | DNS |
| crt.sh | Certificate Transparency |
| Shodan | Internet exposure |
| Censys | Internet/certificate intelligence |
| GitHub | Public code |
| Wayback Machine | Historical pages |
| gau | Historical URLs |
| theHarvester | Public-source aggregation |
| SpiderFoot | OSINT automation |

---

## Automation vs Manual Analysis

Automation:

```text
Collects faster
```

Human analysis:

```text
Understands relationships
```

The strongest workflow combines both.

Avoid:

```text
Run SpiderFoot
     ↓
Export Everything
     ↓
Call It OSINT
```

Instead:

```text
Define Question
     ↓
Collect
     ↓
Verify
     ↓
Correlate
     ↓
Document
```

---

## Red Team Perspective

OSINT should help answer:

```text
What does the organization expose publicly?

How are its technologies connected?

Which assets deserve authorized technical validation?
```

The goal is actionable intelligence, not maximum data collection.

---

## Common Beginner Mistakes

- Collecting unnecessary personal data.
- Confusing assumptions with facts.
- Trusting one source.
- Ignoring historical context.
- Treating third-party systems as in scope.
- Running tools without defining intelligence requirements.
- Failing to record sources.
- Not assigning confidence to findings.

---

## Interview Questions

### What is OSINT?

Intelligence derived from publicly available information.

### OSINT vs reconnaissance?

OSINT focuses on public information; reconnaissance can additionally include direct technical interaction.

### Why correlate multiple sources?

It increases confidence and reduces false assumptions.

### Why is data minimization important?

Professional assessments should collect only information necessary for the authorized objective.

### What are common OSINT sources?

DNS, Certificate Transparency, search engines, public code, archives, documents, and Internet-exposure datasets.

---

## Quick Revision

```text
Define Requirements
      ↓
Collect
      ↓
Verify
      ↓
Correlate
      ↓
Analyze
      ↓
Document

Sources:

DNS
CT Logs
Search Engines
GitHub
Archives
Documents
Shodan
Censys

Rule:

Publicly discoverable ≠ Automatically in scope
```

---

## Practice

Practice using:

- Your own domain
- TryHackMe OSINT exercises
- Purpose-built OSINT challenges
- Authorized CTF environments

---

## References

- OWASP Web Security Testing Guide
- ICANN RDAP Documentation
- Certificate Transparency Project
- Shodan Documentation
- Censys Documentation
- GitHub Documentation
- Internet Archive
