# Httpx

> HTTP probing, service validation, and web attack-surface triage for authorized reconnaissance.

## What is httpx?

**httpx** is a fast HTTP toolkit commonly used during reconnaissance to identify responsive web services and collect information about them.

It is especially useful after subdomain discovery.

Conceptually:

```text
Subdomains
    ↓
  httpx
    ↓
Live HTTP/HTTPS Services
    ↓
Web Enumeration
```

---

# Why httpx?

Subdomain tools may discover thousands of names:

```text
www.example.com
api.example.com
old.example.com
dev.example.com
vpn.example.com
```

But:

```text
Discovered Hostname
       !=
Live Web Application
```

httpx helps determine which hosts actually respond over HTTP/HTTPS.

---

# Basic Workflow

```text
Amass / Subfinder / Assetfinder
             ↓
        subdomains.txt
             ↓
            httpx
             ↓
         live.txt
```

This reduces a large reconnaissance dataset into a more useful list of responsive services.

---

# Basic Usage

A common pipeline is:

```bash
cat subdomains.txt | httpx
```

or, depending on the installed ProjectDiscovery version:

```bash
httpx -l subdomains.txt
```

Always check:

```bash
httpx -h
```

for current syntax.

---

# Save Results

```bash
httpx -l subdomains.txt -o live.txt
```

Then:

```bash
cat live.txt
```

---

# What Can httpx Collect?

Depending on options/version, httpx can collect information such as:

```text
URL
Status Code
Page Title
Content Length
Technology
IP Address
Web Server
Redirect
TLS information
```

This makes it useful for:

```text
Validation
Fingerprinting
Prioritization
```

---

# Status Codes

Useful responses include:

```text
200 OK
204 No Content

301 Moved Permanently
302 Found

401 Unauthorized
403 Forbidden

404 Not Found

500 Internal Server Error
502 Bad Gateway
503 Service Unavailable
```

Do not keep only:

```text
200
```

because:

```text
401
403
3xx
```

can identify important applications and protected resources.

---

# Page Titles

Titles can reveal application purpose.

Examples:

```text
Admin Portal
Grafana
Jenkins
Sign In
API Documentation
Dashboard
```

Conceptually:

```text
Host
 ↓
HTTP Response
 ↓
<title>
 ↓
Application Context
```

Titles are useful for triage, not proof of vulnerability.

---

# Technology Detection

httpx can help identify technologies such as:

```text
nginx
Apache
IIS
WordPress
React
Cloudflare
Application frameworks
```

Technology detection is usually based on observable fingerprints.

Therefore:

```text
Fingerprint
   !=
Guaranteed Identification
```

Verify important findings manually.

---

# Status + Title + Technology

A useful reconnaissance record looks conceptually like:

```text
https://admin.example.com
[200]
[Admin Portal]
[nginx]
```

This provides much more context than:

```text
admin.example.com
```

alone.

---

# Redirects

Applications commonly redirect:

```text
HTTP
 ↓
HTTPS
```

or:

```text
/
 ↓
/login
```

Redirect information can reveal:

```text
Authentication portals
Canonical domains
External identity providers
Application structure
```

Always scope-check redirect destinations before active testing.

---

# HTTP and HTTPS

A hostname may expose:

```text
HTTP only
HTTPS only
Both
Different applications on different ports
```

httpx helps determine actual reachable web services.

---

# Ports

Web applications are not limited to:

```text
80
443
```

Common alternate ports include:

```text
8000
8080
8443
8888
3000
5000
```

Port scanning and engagement scope should determine which ports are appropriate to probe.

---

# Input Deduplication

Before probing:

```bash
sort -u subdomains.txt -o subdomains.txt
```

This reduces duplicate requests.

---

# httpx + Subfinder

Common pipeline:

```text
Domain
  ↓
Subfinder
  ↓
Subdomains
  ↓
httpx
  ↓
Live Web Services
```

Conceptually:

```bash
subfinder -d example.com -silent | httpx
```

Use only against authorized scope.

---

# httpx + Amass

```text
Amass
 ↓
Attack-Surface Names
 ↓
httpx
 ↓
Responsive Web Services
```

---

# httpx + Assetfinder

```text
Assetfinder
     ↓
Subdomains
     ↓
httpx
     ↓
Live Applications
```

---

# Merge Multiple Sources

A better workflow can combine:

```text
Amass --------+
              |
Subfinder ----+--> Merge --> sort -u --> httpx
              |
Assetfinder --+
```

Example:

```bash
cat amass.txt subfinder.txt assetfinder.txt |
sort -u > all-subs.txt
```

Then:

```bash
httpx -l all-subs.txt -o live.txt
```

---

# httpx + GAU

GAU provides historical URLs.

httpx helps validate current availability.

```text
GAU
 ↓
Historical URLs
 ↓
httpx
 ↓
Currently Reachable URLs
```

---

# httpx + FFUF

```text
Subdomains
    ↓
httpx
    ↓
Live Web Application
    ↓
FFUF
    ↓
Content Discovery
```

This prevents wasting directory-enumeration traffic on dead hosts.

---

# httpx + Feroxbuster

```text
httpx
  ↓
Live Application
  ↓
Feroxbuster
  ↓
Recursive Content Discovery
```

---

# httpx + Nuclei

A common authorized workflow:

```text
Subdomains
    ↓
httpx
    ↓
Live Targets
    ↓
Nuclei
    ↓
Template-Based Checks
```

Important:

```text
Nuclei Finding
     !=
Confirmed Vulnerability
```

Manually validate important findings.

---

# httpx + Burp / Caido

Once interesting applications are identified:

```text
httpx
 ↓
Interesting Application
 ↓
Burp / Caido
 ↓
Manual Web Testing
```

---

# Filtering

Large target sets can be prioritized by:

```text
Status
Title
Technology
Server
Content length
```

Example conceptual triage:

```text
Admin
Dashboard
Jenkins
Grafana
API
Login
Swagger
```

These are leads, not vulnerabilities.

---

# WAF / CDN Considerations

A host may sit behind:

```text
Cloudflare
AWS CloudFront
Akamai
Fastly
Other reverse proxies/CDNs
```

The visible IP/server may therefore not represent the actual origin infrastructure.

Do not treat CDN/provider infrastructure as automatically in scope.

---

# False Positives

Potential causes include:

```text
Wildcard DNS
Default web pages
CDN responses
Catch-all virtual hosts
Generic reverse proxies
Shared hosting
```

Therefore:

```text
Responsive Host
     !=
Confirmed Organization Application
```

Validate ownership and scope.

---

# Recon Methodology

```text
1. Collect subdomains

2. Deduplicate

3. Validate DNS

4. Probe HTTP/HTTPS

5. Record status/title/technology

6. Follow redirects carefully

7. Scope-check discovered destinations

8. Prioritize applications

9. Perform content discovery

10. Manually analyze
```

---

# Operational Considerations

httpx can generate many HTTP requests.

Across thousands of hosts:

```text
Hosts
   ×
Ports
   ×
Protocols
   =
Large Request Volume
```

Respect:

```text
Rate limits
Concurrency limits
Scope
Rules of engagement
Third-party infrastructure
```

---

# Key Takeaway

httpx sits between:

```text
ASSET DISCOVERY
       ↓
      httpx
       ↓
WEB APPLICATION TESTING
```

Its main role is:

```text
Discovered Host
      ↓
Is HTTP/HTTPS alive?
      ↓
What does it appear to be?
      ↓
Should it be investigated further?
```

Think of httpx primarily as a **web attack-surface validation and triage tool**, not a vulnerability scanner.
