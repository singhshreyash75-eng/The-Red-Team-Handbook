# Amass

> OWASP Amass fundamentals for attack-surface mapping, DNS enumeration, and authorized reconnaissance.

## What is Amass?

**OWASP Amass** is an attack-surface mapping and reconnaissance framework.

It is primarily useful for discovering and correlating information about:

```text
Domains
Subdomains
DNS records
IP addresses
Autonomous Systems
Network infrastructure
Relationships between assets
```

Conceptually:

```text
Root Domain
    |
    v
   Amass
    |
    +-- Subdomains
    +-- DNS Records
    +-- IP Addresses
    +-- Infrastructure
```

---

# Primary Use

Suppose the authorized scope contains:

```text
example.com
```

The organization may also operate:

```text
www.example.com
api.example.com
dev.example.com
vpn.example.com
mail.example.com
portal.example.com
```

Reconnaissance attempts to map these assets.

---

# Passive vs Active Reconnaissance

Amass can perform different kinds of discovery.

## Passive

Passive reconnaissance relies primarily on external information sources rather than directly probing every target.

Conceptually:

```text
Public Data Sources
      |
      +-- DNS datasets
      +-- Certificate data
      +-- Search sources
      +-- Internet datasets
      |
      v
Subdomain Discovery
```

Advantages:

```text
Lower target interaction
Broad discovery
Useful early in recon
```

---

## Active

Active reconnaissance may interact directly with target infrastructure.

Conceptually:

```text
Amass
  |
  v
DNS / Target Infrastructure
  |
  v
Additional discoveries
```

This can produce more traffic and should only be used where the engagement scope permits it.

---

# Basic Enumeration Concept

Amass syntax varies somewhat by release, so check:

```bash
amass -h
```

and the relevant subcommand help.

A commonly encountered enumeration pattern is:

```bash
amass enum -d example.com
```

This asks Amass to enumerate assets associated with the domain.

Use only against domains within your authorized scope.

---

# Passive Enumeration

A commonly encountered pattern:

```bash
amass enum -passive -d example.com
```

Conceptually:

```text
example.com
    |
Passive Sources
    |
    v
Discovered Subdomains
```

---

# Save Output

Recon data should normally be saved.

Example:

```bash
amass enum -passive -d example.com -o amass.txt
```

Then:

```bash
cat amass.txt
```

This makes later processing easier.

---

# Multiple Domains

Large engagements may contain several root domains.

Conceptually:

```text
Scope
 |
 +-- example.com
 +-- example.net
 +-- example.org
```

Keep results separated or clearly tagged so assets are not accidentally attributed to the wrong scope.

---

# DNS Fundamentals

Amass becomes easier to understand when DNS is clear.

Common records:

```text
A       IPv4 address
AAAA    IPv6 address
CNAME   Alias
MX      Mail server
NS      Name server
TXT     Text data
SOA     Zone authority information
```

Example:

```text
api.example.com
       |
       A
       |
       v
203.0.113.10
```

---

# Certificate Transparency

TLS certificates can expose subdomain names.

Conceptually:

```text
TLS Certificate
      |
Subject / SANs
      |
      v
api.example.com
vpn.example.com
mail.example.com
```

Certificate Transparency data is therefore useful during passive asset discovery.

---

# DNS Resolution

A discovered name is not automatically a live application.

Example:

```text
old.example.com
```

may exist in historical/public data but no longer resolve.

Therefore:

```text
Discovered Name
      ↓
DNS Resolution
      ↓
Resolved Host
      ↓
HTTP Probing
```

This is why Amass is often combined with other tools.

---

# Recon Pipeline

Example authorized workflow:

```text
Root Domain
     ↓
Amass
     ↓
Subdomains
     ↓
DNS Resolution
     ↓
httpx
     ↓
Live HTTP Services
     ↓
Further Web Enumeration
```

---

# Amass + httpx

Conceptually:

```text
Amass
 |
Subdomains
 |
 v
httpx
 |
Live Web Services
```

Example pipeline shape:

```bash
cat amass.txt | httpx
```

Exact flags should be selected according to the installed tool versions and assessment scope.

---

# Amass + Nuclei

A common workflow is:

```text
Amass
   ↓
Assets
   ↓
httpx
   ↓
Live HTTP Targets
   ↓
Nuclei
   ↓
Template-based checks
```

Nuclei findings still require validation.

---

# Amass + Burp Suite

For interesting applications:

```text
Amass
   ↓
Subdomain
   ↓
Live Application
   ↓
Burp Suite
   ↓
Manual Web Testing
```

Automated recon finds candidates.

Manual analysis determines actual security impact.

---

# Asset Correlation

The value of Amass is not only finding names.

It can help understand relationships:

```text
Domain
  |
Subdomain
  |
IP Address
  |
Network
  |
Organization
```

This is why Amass is often described as an:

```text
Attack Surface Mapping
```

tool rather than only a subdomain finder.

---

# False Positives / Stale Data

Passive sources may contain:

```text
Old DNS records
Expired infrastructure
Third-party assets
Historical subdomains
Unrelated certificate names
```

Therefore:

```text
Discovery
   !=
Confirmed Current Asset
```

Always validate ownership and current resolution.

---

# Scope Validation

Suppose:

```text
example.com
```

is authorized.

A discovered hostname may point to infrastructure operated by:

```text
AWS
Azure
Cloudflare
SaaS provider
Third-party vendor
```

Do not automatically assume the underlying provider infrastructure is authorized for testing.

Scope is defined by the engagement, not by what a reconnaissance tool discovers.

---

# Useful Processing

Remove duplicates:

```bash
sort -u amass.txt
```

Count findings:

```bash
wc -l amass.txt
```

Search for interesting names:

```bash
grep -Ei 'dev|test|stage|api|admin|vpn' amass.txt
```

These are triage techniques, not proof of vulnerability.

---

# Interesting Naming Patterns

During authorized reconnaissance, names such as these may deserve review:

```text
dev
test
stage
staging
api
admin
portal
vpn
internal
old
backup
jenkins
git
grafana
```

But:

```text
Interesting hostname
    !=
Vulnerability
```

---

# Recon Methodology

Use a structured workflow:

```text
1. Define scope

2. Identify root domains

3. Passive enumeration

4. Validate DNS

5. Active enumeration if authorized

6. Deduplicate results

7. Identify live services

8. Classify technologies

9. Prioritize interesting assets

10. Manually validate
```

---

# Operational Considerations

Reconnaissance can create:

```text
DNS traffic
HTTP traffic
API queries
Large result sets
```

Respect:

```text
Scope
Rate limits
Rules of engagement
Third-party boundaries
```

---

# Amass vs Subfinder

Both can perform subdomain discovery.

Conceptually:

```text
Subfinder
    |
Fast passive subdomain discovery
```

while Amass provides broader:

```text
Attack-surface mapping
DNS relationships
Infrastructure correlation
```

They can complement each other.

---

# Amass vs Assetfinder

```text
Assetfinder
     |
Simple asset/subdomain discovery
```

```text
Amass
     |
More comprehensive attack-surface mapping
```

Combining sources can increase coverage.

---

# Recommended Pipeline

```text
Amass
   +
Subfinder
   +
Assetfinder
       ↓
Merge Results
       ↓
sort -u
       ↓
Resolve
       ↓
httpx
       ↓
Live Applications
```

---

# Key Takeaway

Amass is best understood as:

```text
DOMAIN
   ↓
DISCOVERY
   ↓
DNS
   ↓
INFRASTRUCTURE
   ↓
RELATIONSHIPS
   ↓
ATTACK SURFACE
```

Do not treat every discovered hostname as a target.

Always apply:

```text
Discovery
    ↓
Scope Validation
    ↓
Resolution
    ↓
Service Validation
    ↓
Manual Analysis
```
