# Assetfinder

> Fast domain and subdomain discovery notes for authorized reconnaissance.

## What is Assetfinder?

**Assetfinder** is a lightweight reconnaissance tool commonly used to discover domains and subdomains associated with a target domain.

Conceptually:

```text
example.com
    |
Assetfinder
    |
    v
Subdomains / Related Assets
```

It is particularly useful at the beginning of an authorized reconnaissance workflow.

---

# Main Purpose

Suppose the scope contains:

```text
example.com
```

Assetfinder may help discover names such as:

```text
www.example.com
api.example.com
mail.example.com
dev.example.com
portal.example.com
```

These become candidates for further validation.

---

# Basic Usage

A commonly encountered syntax is:

```bash
assetfinder example.com
```

Depending on the tool version, options may be available to restrict output.

Always check:

```bash
assetfinder -h
```

if uncertain about installed-version behavior.

---

# Subdomains Only

A frequently used pattern is:

```bash
assetfinder --subs-only example.com
```

Conceptually:

```text
example.com
    ↓
Assetfinder
    ↓
Only names under example.com
```

This can reduce unrelated results.

---

# Save Results

```bash
assetfinder --subs-only example.com > assetfinder.txt
```

Review:

```bash
cat assetfinder.txt
```

Count:

```bash
wc -l assetfinder.txt
```

---

# Deduplicate

Recon tools may return duplicate results.

Use:

```bash
sort -u assetfinder.txt
```

Or:

```bash
sort -u assetfinder.txt -o assetfinder.txt
```

Now the file contains unique entries.

---

# Assetfinder Is Discovery, Not Validation

Important:

```text
Assetfinder Result
      !=
Live Host
```

A hostname may be:

```text
Historical
Offline
Unresolvable
Third-party
Out of scope
```

Therefore:

```text
Discovery
   ↓
DNS Validation
   ↓
Service Validation
```

is required.

---

# Passive Reconnaissance

Assetfinder is primarily useful for gathering information from external/public sources rather than brute-forcing every possible hostname.

Conceptually:

```text
Public Sources
     ↓
Assetfinder
     ↓
Known Asset Names
```

This makes it useful during the early reconnaissance phase.

---

# Assetfinder + Amass

Different discovery tools may use different data sources.

Therefore:

```text
Assetfinder
     +
Amass
     ↓
Better Coverage
```

Example workflow:

```bash
assetfinder --subs-only example.com > assetfinder.txt
amass enum -passive -d example.com -o amass.txt
```

Merge:

```bash
cat assetfinder.txt amass.txt | sort -u > subdomains.txt
```

---

# Assetfinder + Subfinder

Similarly:

```text
Assetfinder
     +
Subfinder
     ↓
Combined Subdomain List
```

Example:

```bash
cat assetfinder.txt subfinder.txt | sort -u > subdomains.txt
```

---

# Combining Three Sources

Conceptually:

```text
Amass --------+
              |
Subfinder ----+--> Merge --> Deduplicate
              |
Assetfinder --+
```

Example:

```bash
cat amass.txt subfinder.txt assetfinder.txt | sort -u > all-subs.txt
```

This is a common recon principle:

```text
Multiple data sources
       ↓
Higher discovery coverage
```

---

# DNS Validation

Once names are collected:

```text
all-subs.txt
     ↓
DNS Resolution
     ↓
Current Assets
```

Not every historical hostname will resolve.

DNS validation prevents wasting time on stale results.

---

# Assetfinder + httpx

After discovery:

```text
Assetfinder
     ↓
Subdomains
     ↓
httpx
     ↓
Live HTTP/HTTPS services
```

Example pipeline shape:

```bash
cat assetfinder.txt | httpx
```

This identifies which discovered names expose reachable HTTP services from the assessment environment.

---

# Save Live Targets

Conceptually:

```bash
cat assetfinder.txt | httpx > live.txt
```

Then:

```bash
cat live.txt
```

The resulting applications can be prioritized for manual web testing.

---

# Assetfinder + Burp Suite

Workflow:

```text
Assetfinder
      ↓
Subdomain
      ↓
httpx
      ↓
Live Application
      ↓
Burp Suite
      ↓
Manual Analysis
```

Assetfinder discovers.

Burp analyzes HTTP behavior.

---

# Assetfinder + Nuclei

Another workflow:

```text
Assetfinder
      ↓
Subdomains
      ↓
httpx
      ↓
Live Targets
      ↓
Nuclei
      ↓
Template-based checks
```

Automated findings still require manual validation.

---

# Interesting Subdomains

Names may help prioritize assets.

Examples:

```text
api.example.com
admin.example.com
dev.example.com
test.example.com
stage.example.com
vpn.example.com
git.example.com
jenkins.example.com
```

However:

```text
Interesting Name
      !=
Security Finding
```

The name is only a lead.

---

# Scope

This is critical.

Suppose your authorized scope is:

```text
*.example.com
```

Assetfinder may discover related domains or third-party services.

Do not automatically test them.

Use:

```text
Discovered Asset
      ↓
Scope Check
      ↓
Authorized?
     / \
   Yes  No
    |    |
 Test   Stop
```

---

# Third-Party Infrastructure

A hostname might resolve to:

```text
AWS
Azure
Cloudflare
GitHub
SaaS provider
CDN
```

Testing the underlying provider or unrelated tenants is not automatically authorized simply because the organization's hostname points there.

---

# Recon Data Management

For larger engagements, maintain files such as:

```text
assetfinder.txt
amass.txt
subfinder.txt
all-subs.txt
live.txt
```

Pipeline:

```text
Raw Sources
     ↓
Normalize
     ↓
Deduplicate
     ↓
Resolve
     ↓
Probe
     ↓
Prioritize
```

---

# Filtering

Example:

```bash
grep -Ei 'api|admin|dev|test|stage' all-subs.txt
```

This can help prioritize review.

But avoid discarding everything else; security-relevant services often have ordinary names.

---

# Assetfinder vs Gobuster

They solve different problems.

```text
Assetfinder
     |
Subdomain / asset discovery
```

versus:

```text
Gobuster
     |
Content / DNS brute-force enumeration
```

Assetfinder typically relies on known/passive sources.

Gobuster can actively test candidate names/paths.

---

# Assetfinder vs Amass

```text
Assetfinder
     |
Simple + fast discovery
```

```text
Amass
     |
Broader attack-surface mapping
```

Use both when broader coverage is needed.

---

# Assetfinder vs Subfinder

Both are commonly used for passive subdomain discovery.

The main lesson is not:

```text
Which one is always best?
```

Instead:

```text
Different sources
      ↓
Different findings
      ↓
Combine + deduplicate
```

---

# Recommended Recon Pipeline

```text
                ROOT DOMAIN
                     |
          +----------+----------+
          |          |          |
       Amass     Subfinder   Assetfinder
          |          |          |
          +----------+----------+
                     |
                  Merge
                     |
                  sort -u
                     |
               DNS Validation
                     |
                   httpx
                     |
              Live Web Targets
                     |
          Manual / Scoped Testing
```

---

# Quick Commands

Discovery:

```bash
assetfinder --subs-only example.com
```

Save:

```bash
assetfinder --subs-only example.com > assetfinder.txt
```

Deduplicate:

```bash
sort -u assetfinder.txt -o assetfinder.txt
```

Probe:

```bash
cat assetfinder.txt | httpx
```

---

# Key Takeaway

Assetfinder's role is:

```text
ROOT DOMAIN
     ↓
PASSIVE DISCOVERY
     ↓
SUBDOMAINS
     ↓
VALIDATION
```

It does **not** answer:

```text
Is this host vulnerable?
```

It answers:

```text
What assets might exist?
```

The proper workflow is:

```text
Discover
   ↓
Deduplicate
   ↓
Validate DNS
   ↓
Check Scope
   ↓
Probe Services
   ↓
Analyze
```
