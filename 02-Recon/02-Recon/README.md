# 🔎 Reconnaissance

> Reconnaissance is the process of collecting information about a target before deeper security testing begins. Good recon reduces guesswork and helps identify the systems, technologies, and attack surfaces that deserve attention.

---

## Overview

Recon is usually the first technical phase of an authorized penetration test or bug bounty assessment.

The objective is to answer questions such as:

- What assets belong to the target?
- What domains and subdomains exist?
- Which hosts are reachable?
- What technologies are exposed?
- What applications and APIs exist?
- Are there development or staging environments?
- What historical information is publicly available?
- Where should deeper enumeration begin?

A strong recon process turns:

```text
example.com
```

into an organized attack-surface map.

---

## Recon vs Enumeration

These concepts overlap, but they are not identical.

### Reconnaissance

Focuses primarily on discovering the attack surface.

Examples:

- Domains
- Subdomains
- IP ranges
- Technologies
- Public information
- Historical URLs
- JavaScript files

### Enumeration

Interacts more deeply with discovered services.

Examples:

- SMB shares
- LDAP information
- FTP access
- DNS configuration
- Web directories
- Service versions

A simplified workflow is:

```text
Target
   ↓
Recon
   ↓
Asset Discovery
   ↓
Enumeration
   ↓
Vulnerability Analysis
   ↓
Validation
```

---

## Passive vs Active Recon

### Passive Recon

Information is collected without directly probing the target infrastructure where possible.

Examples:

- Search Engines
- Certificate Transparency
- WHOIS/RDAP
- Public GitHub repositories
- Internet archives
- Public DNS datasets

Advantages:

- Low interaction
- Broad historical coverage
- Useful for initial mapping

---

### Active Recon

The tester directly interacts with in-scope systems.

Examples:

- DNS queries
- HTTP probing
- Technology fingerprinting
- Content discovery
- Virtual-host discovery

Active techniques should only be performed against systems covered by the engagement's authorization and scope.

---

## Module Structure

```text
02-Recon/
│
├── README.md
├── Recon-Methodology.md
├── Passive-Recon.md
├── Active-Recon.md
├── Subdomain-Enumeration.md
├── Content-Discovery.md
├── Parameter-Discovery.md
├── JavaScript-Recon.md
├── GitHub-Recon.md
├── Google-Dorking.md
├── OSINT.md
└── Virtual-Hosts.md
```

---

## Recon Workflow

```text
Define Scope
     ↓
Passive Recon
     ↓
Domain & Subdomain Discovery
     ↓
DNS Resolution
     ↓
HTTP Probing
     ↓
Technology Fingerprinting
     ↓
Content Discovery
     ↓
Parameter Discovery
     ↓
JavaScript Analysis
     ↓
Historical URL Collection
     ↓
Prioritize Interesting Assets
     ↓
Enumeration
```

---

## Important Recon Tools

| Tool | Purpose |
|------|---------|
| `whois` / RDAP | Registration information |
| `dig` | DNS queries |
| `Subfinder` | Passive subdomain discovery |
| `Amass` | Asset and DNS discovery |
| `dnsx` | DNS resolution |
| `httpx` | HTTP service probing |
| `Katana` | Web crawling |
| `ffuf` | Content/vhost discovery |
| `gau` | Historical/known URL collection |
| `waybackurls` | Wayback Machine URLs |
| `Arjun` | HTTP parameter discovery |

> Tool behavior and flags change over time. Always check the current official documentation before using a command in an assessment.

---

## What Makes an Asset Interesting?

Examples include:

```text
admin.example.com
api.example.com
dev.example.com
staging.example.com
internal.example.com
old.example.com
beta.example.com
vpn.example.com
```

Also prioritize:

- Login portals
- Admin interfaces
- APIs
- File-upload functionality
- Legacy applications
- Development environments
- Unusual technologies
- Forgotten subdomains

---

## Recon Notes

During an engagement, maintain structured notes such as:

```text
Domain:
example.com

Subdomains:
api.example.com
dev.example.com
admin.example.com

Technologies:
nginx
React
Node.js

Interesting Endpoints:
/api/
/admin/
/upload/

Notes:
Staging environment discovered
```

Good documentation prevents duplicated work and makes later enumeration much faster.

---

## Recommended Practice

### TryHackMe

Useful topics:

- Passive Reconnaissance
- Active Reconnaissance
- DNS
- Web Enumeration

### Hack The Box Academy

Useful modules:

- Information Gathering
- Web Information Gathering
- Footprinting

### PortSwigger Web Security Academy

Useful for understanding the web technologies and vulnerabilities discovered after recon.

---

## Quick Revision

```text
Recon = Discover attack surface

Passive Recon = Minimal/no direct interaction

Active Recon = Direct interaction with in-scope assets

Subdomains = Expand attack surface

DNS = Map names to infrastructure

HTTP probing = Identify web services

JS Recon = Discover endpoints and application behavior

Historical URLs = Find old or forgotten functionality
```

---

## References

- OWASP Web Security Testing Guide
- NIST SP 800-115
- ProjectDiscovery Documentation
- OWASP Amass Documentation
- Hack The Box Academy
- TryHackMe
