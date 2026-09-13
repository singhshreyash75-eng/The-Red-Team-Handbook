# Subfinder

> ProjectDiscovery Subfinder fundamentals for passive subdomain enumeration and authorized reconnaissance.

## What is Subfinder?

**Subfinder** is a fast passive subdomain-discovery tool developed by ProjectDiscovery.

It gathers subdomains from multiple external data sources.

Conceptually:

```text
Root Domain
    ↓
Public / Passive Sources
    ↓
Subfinder
    ↓
Subdomains
```

Example:

```text
example.com
    |
    +-- www.example.com
    +-- api.example.com
    +-- dev.example.com
    +-- vpn.example.com
```

---

# Passive Reconnaissance

Subfinder is primarily designed for:

```text
Passive Subdomain Enumeration
```

Instead of brute-forcing large numbers of DNS names, it gathers information from existing external sources.

Conceptually:

```text
Internet Data Sources
       ↓
Subfinder
       ↓
Known Subdomains
```

---

# Why Subdomains Matter

An organization's main website may be:

```text
www.example.com
```

but additional applications may exist at:

```text
api.example.com
admin.example.com
dev.example.com
staging.example.com
vpn.example.com
mail.example.com
```

Therefore:

```text
Root Domain
    !=
Complete Attack Surface
```

---

# Basic Usage

For an authorized domain:

```bash
subfinder -d example.com
```

Quiet output:

```bash
subfinder -d example.com -silent
```

Check installed-version help:

```bash
subfinder -h
```

---

# Save Results

```bash
subfinder -d example.com -silent -o subdomains.txt
```

Review:

```bash
cat subdomains.txt
```

Count:

```bash
wc -l subdomains.txt
```

---

# Multiple Domains

For multiple authorized root domains, a domain-list workflow can be used where supported by the installed version.

Conceptually:

```text
domains.txt
    ↓
Subfinder
    ↓
All Discovered Subdomains
```

Check:

```bash
subfinder -h
```

for current list-input syntax.

---

# Data Sources

Passive subdomain enumeration can draw from sources such as:

```text
Certificate transparency
Search/index services
DNS datasets
Security datasets
Public APIs
```

Exact providers supported by Subfinder change over time.

---

# API Keys

Some passive sources provide better results when API credentials are configured.

Conceptually:

```text
Subfinder
   |
   +--> Free Source
   |
   +--> API Source
   |
   +--> Certificate Source
```

Protect reconnaissance API keys as credentials.

Do not commit them into:

```text
Git repositories
Public configuration
Screenshots
```

---

# Passive Does Not Mean Invisible

Important:

```text
Passive
!=
Zero external interaction
```

The tool still communicates with external data providers.

The distinction is that it generally avoids directly brute-forcing/probing the target infrastructure as its primary discovery mechanism.

---

# Results Need Validation

A Subfinder result may be:

```text
Current
Historical
Offline
Third-party
Stale
```

Therefore:

```text
Subfinder Result
      !=
Live Host
```

Follow with validation.

---

# DNS Validation

Conceptually:

```text
Subfinder
    ↓
Subdomains
    ↓
DNS Resolution
    ↓
Currently Resolving Names
```

Historical datasets frequently contain old names.

---

# Subfinder + httpx

One of the most useful pipelines:

```text
Subfinder
    ↓
Subdomains
    ↓
httpx
    ↓
Live HTTP Services
```

Example:

```bash
subfinder -d example.com -silent | httpx
```

Save:

```bash
subfinder -d example.com -silent |
httpx > live.txt
```

Use only within authorized scope.

---

# Why httpx After Subfinder?

Suppose Subfinder finds:

```text
500 subdomains
```

but only:

```text
80
```

respond to HTTP/HTTPS.

Then:

```text
500 discovered names
        ↓
httpx
        ↓
80 live web applications
```

This makes later testing much more efficient.

---

# Subfinder + Nuclei

Common conceptual pipeline:

```text
Subfinder
    ↓
httpx
    ↓
Nuclei
```

Meaning:

```text
Discover
 ↓
Validate
 ↓
Template Checks
```

Always manually validate significant scanner findings.

---

# Subfinder + Katana

```text
Subfinder
    ↓
httpx
    ↓
Live Applications
    ↓
Katana
    ↓
Endpoints
```

This expands:

```text
Host Attack Surface
```

into:

```text
Application Attack Surface
```

---

# Subfinder + GAU

Historical URL discovery can be performed after subdomain discovery.

```text
Subfinder
    ↓
Subdomains
    ↓
GAU
    ↓
Historical URLs
```

This can uncover endpoints no longer linked by the current application.

---

# Subfinder + FFUF

Conceptually:

```text
Subfinder
 ↓
Live Host
 ↓
FFUF
 ↓
Hidden Content
```

Do not run large fuzzing lists against every discovered hostname without first validating and prioritizing targets.

---

# Subfinder + Burp / Caido

Final manual workflow:

```text
Subfinder
    ↓
httpx
    ↓
Interesting Application
    ↓
Burp / Caido
    ↓
Manual Testing
```

---

# Combining Discovery Tools

No passive source has perfect coverage.

Therefore:

```text
Subfinder ----+
              |
Amass --------+--> Merge
              |
Assetfinder --+
```

Then:

```bash
cat subfinder.txt amass.txt assetfinder.txt |
sort -u > all-subs.txt
```

This produces a deduplicated dataset.

---

# Why Combine Tools?

Different tools use:

```text
Different APIs
Different datasets
Different discovery logic
```

Therefore:

```text
Tool A finds X

Tool B finds Y

Combined finds X + Y
```

---

# Deduplication

Always normalize results:

```bash
sort -u subdomains.txt -o subdomains.txt
```

Large recon pipelines can otherwise waste significant requests on duplicate targets.

---

# Interesting Names

Potentially useful triage terms:

```text
admin
api
dev
test
stage
staging
vpn
portal
git
jenkins
grafana
internal
```

Example:

```bash
grep -Ei 'admin|api|dev|stage|vpn' subdomains.txt
```

But:

```text
Interesting Hostname
      !=
Vulnerability
```

---

# Certificate Transparency

TLS certificates can contain:

```text
Subject Alternative Names
```

such as:

```text
api.example.com
mail.example.com
vpn.example.com
```

Certificate Transparency ecosystems therefore provide valuable passive discovery data.

---

# Wildcard Domains

Some organizations configure wildcard DNS:

```text
*.example.com
```

which may cause arbitrary names to resolve.

Passive discovery reduces some brute-force noise, but all results still require validation and ownership checking.

---

# Third-Party Services

Subdomains may point to:

```text
Cloudflare
AWS
Azure
GitHub
CDNs
SaaS applications
Support platforms
Identity providers
```

The organization's hostname being in scope does not automatically authorize testing of unrelated underlying provider infrastructure.

---

# Scope Validation

For every result:

```text
Discovered Host
      ↓
Scope?
   /      \
 Yes      No
  |        |
Continue   Stop
```

Scope is defined by the rules of engagement.

---

# Recon Workflow

A strong methodology:

```text
1. Identify authorized root domains

2. Run passive enumeration

3. Merge multiple sources

4. Deduplicate

5. Validate DNS

6. Probe HTTP/HTTPS

7. Identify technologies

8. Crawl applications

9. Perform scoped content discovery

10. Manually validate
```

---

# Subfinder vs Amass

```text
Subfinder
    |
Fast passive subdomain discovery
```

```text
Amass
    |
Broader DNS / infrastructure /
attack-surface mapping
```

Both are useful.

---

# Subfinder vs Assetfinder

Both provide lightweight passive discovery.

Rather than choosing only one:

```text
Multiple Sources
      ↓
Merge
      ↓
Deduplicate
```

often provides better coverage.

---

# Subfinder vs Gobuster DNS

Important distinction:

```text
Subfinder
    |
Passive discovery
```

```text
Gobuster DNS
    |
Active candidate DNS enumeration
```

Passive reconnaissance is usually a sensible first stage before noisier active enumeration.

---

# Recommended Pipeline

```text
                DOMAIN
                  |
        +---------+---------+
        |         |         |
   Subfinder    Amass   Assetfinder
        |         |         |
        +---------+---------+
                  |
                Merge
                  |
               sort -u
                  |
                 DNS
                  |
                httpx
                  |
            Live Web Apps
                  |
          Katana / GAU
                  |
              Endpoints
                  |
         FFUF / Feroxbuster
                  |
            Nuclei Checks
                  |
           Burp / Caido
                  |
          Manual Validation
```

---

# Key Takeaway

Subfinder's job is:

```text
ROOT DOMAIN
     ↓
PASSIVE SOURCES
     ↓
SUBDOMAIN DISCOVERY
```

It does not tell you:

```text
Is this application vulnerable?
```

It answers:

```text
What additional hostnames
might belong to the authorized
attack surface?
```

Always follow:

```text
DISCOVER
   ↓
DEDUPLICATE
   ↓
VALIDATE
   ↓
SCOPE CHECK
   ↓
PROBE
   ↓
ANALYZE
```
