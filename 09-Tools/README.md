# Tools

> Core red-team, reconnaissance, web-security, password-auditing, and network-analysis tools for authorized security assessments.

## Module Overview

This module covers tools used across different stages of a security assessment.

The overall workflow can be understood as:

```text
Asset Discovery
      ↓
Service Validation
      ↓
Endpoint Discovery
      ↓
Automated Checks
      ↓
Manual Testing
      ↓
Credential Auditing
      ↓
Network Analysis
```

---

# Files

```text
Amass.md
Assetfinder.md
Burp-Suite.md
Caido.md
FFUF.md
Feroxbuster.md
GAU.md
Gobuster.md
Hashcat.md
Httpx.md
Hydra.md
John-The-Ripper.md
Katana.md
Nuclei.md
README.md
Subfinder.md
Tcpdump.md
Waybackurls.md
Wireshark.md
```

---

# Tool Categories

These tools can be grouped into:

```text
Reconnaissance
Web Enumeration
Web Testing
Vulnerability Detection
Password Auditing
Network Analysis
```

---

# Reconnaissance Tools

Important recon tools:

```text
Amass
Assetfinder
Subfinder
GAU
Waybackurls
```

Their goal is primarily:

```text
Discover Attack Surface
```

---

# Amass

Used for:

```text
Subdomain discovery
DNS enumeration
Infrastructure mapping
Attack-surface relationships
```

Conceptually:

```text
Domain
 ↓
Amass
 ↓
Subdomains + Infrastructure
```

---

# Assetfinder

Lightweight asset/subdomain discovery.

```text
Domain
 ↓
Assetfinder
 ↓
Related Domains / Subdomains
```

Useful during early passive reconnaissance.

---

# Subfinder

Fast passive subdomain enumeration.

```text
Domain
 ↓
Public Data Sources
 ↓
Subfinder
 ↓
Subdomains
```

Commonly used as one of the first tools in a recon pipeline.

---

# GAU

**GAU = Get All URLs**

Used for:

```text
Historical URL discovery
Endpoints
Parameters
Old application paths
```

Conceptually:

```text
Domain
 ↓
Historical Sources
 ↓
GAU
 ↓
URLs
```

---

# Waybackurls

Retrieves URLs associated with domains from historical web-archive data.

Useful for discovering:

```text
Old endpoints
Historical parameters
Removed pages
JavaScript files
API paths
```

---

# HTTP Validation

After discovering subdomains:

```text
Subdomains
     ↓
httpx
     ↓
Live HTTP Services
```

httpx helps determine:

```text
Which hosts respond?
Which status code?
Which title?
Which technology?
```

---

# Content Discovery

Tools include:

```text
FFUF
Gobuster
Feroxbuster
Katana
```

These discover additional web attack surface.

---

# FFUF

Flexible HTTP fuzzing using:

```text
FUZZ
```

Conceptually:

```text
Wordlist
 ↓
FUZZ
 ↓
HTTP Request
 ↓
Interesting Responses
```

Useful for:

```text
Directories
Files
Parameters
VHosts
Controlled HTTP fuzzing
```

---

# Gobuster

Provides enumeration modes for areas such as:

```text
Directories
DNS
Virtual Hosts
```

Conceptually:

```text
Wordlist
 ↓
Candidate Resource
 ↓
Server
 ↓
Response
```

---

# Feroxbuster

Focused heavily on:

```text
Recursive Content Discovery
```

Example:

```text
/
|
+-- api/
     |
     +-- v1/
          |
          +-- users/
```

Feroxbuster can automatically recurse into discovered directories.

---

# Katana

Katana is a crawler.

Instead of guessing paths:

```text
Katana
 ↓
Follow application links/resources
 ↓
Discover endpoints
```

Useful for:

```text
Modern applications
JavaScript
APIs
Forms
Parameters
```

---

# Crawler vs Fuzzer

Important distinction:

```text
Katana
=
Follow exposed application resources
```

versus:

```text
FFUF / Gobuster
=
Guess candidate resources
```

Use both for broader coverage.

---

# Historical vs Current URLs

```text
Katana
 |
Current application
```

```text
GAU / Waybackurls
 |
Historical application data
```

Together:

```text
Current
+
Historical
=
Better Endpoint Coverage
```

---

# Web Proxy Tools

Two tools in this module:

```text
Burp Suite
Caido
```

Both allow:

```text
Capture HTTP traffic
Inspect requests
Modify requests
Replay requests
Analyze responses
```

---

# Burp Suite

Conceptually:

```text
Browser
 ↓
Burp
 ↓
Application
```

One of the most important components:

```text
Repeater
```

Workflow:

```text
Capture
 ↓
Send to Repeater
 ↓
Modify
 ↓
Resend
 ↓
Analyze
```

---

# Caido

Caido provides a modern intercepting-proxy workflow.

Conceptually:

```text
Browser
 ↓
Caido
 ↓
Server
```

Core workflow:

```text
Capture
 ↓
Inspect
 ↓
Replay
 ↓
Compare
```

---

# Nuclei

Nuclei provides:

```text
Template-Based Security Checks
```

Architecture:

```text
Target
 +
Template
 ↓
Request
 ↓
Matcher
 ↓
Potential Finding
```

Important:

```text
Nuclei Match
!=
Confirmed Vulnerability
```

Always manually validate significant findings.

---

# Password Auditing Tools

This module contains:

```text
Hashcat
John the Ripper
Hydra
```

---

# Hashcat

Primarily:

```text
Offline Password Auditing
```

Conceptually:

```text
Hash
 +
Candidates
 ↓
Offline Verification
```

Hashcat is particularly well known for GPU acceleration.

---

# John the Ripper

Also primarily:

```text
Offline Password Auditing
```

Strong areas include:

```text
Format handling
Rules
Conversion utilities
Password auditing
```

---

# Hydra

Hydra differs fundamentally:

```text
Hydra
=
Online Authentication Testing
```

Conceptually:

```text
Credential Candidate
 ↓
Remote Login Service
 ↓
Success / Failure
```

Because it interacts with real authentication systems, rate limits and lockout policies are critical.

---

# Password Tool Comparison

```text
Hashcat
   |
Offline
   |
GPU-oriented


John
   |
Offline
   |
Flexible formats


Hydra
   |
Online
   |
Remote authentication
```

---

# Network Analysis

Tools:

```text
Tcpdump
Wireshark
```

Both analyze network traffic.

---

# Tcpdump

Command-line packet capture and analysis.

```text
Network Interface
      ↓
Tcpdump
      ↓
Packets
```

Excellent for:

```text
Servers
SSH sessions
Quick troubleshooting
Saving PCAP files
```

---

# Wireshark

Graphical packet-analysis tool.

```text
Packets
 ↓
Protocol Decoding
 ↓
Filters
 ↓
Detailed Analysis
```

Excellent for:

```text
Deep protocol inspection
PCAP analysis
Network troubleshooting
Traffic visualization
```

---

# Tcpdump vs Wireshark

```text
Tcpdump
=
CLI packet capture
```

```text
Wireshark
=
GUI packet analysis
```

A common workflow:

```text
Remote Server
     ↓
tcpdump
     ↓
capture.pcap
     ↓
Wireshark
     ↓
Detailed Analysis
```

---

# Complete Recon Pipeline

A strong conceptual pipeline:

```text
                 ROOT DOMAIN
                      |
          +-----------+-----------+
          |           |           |
       Amass      Subfinder   Assetfinder
          |           |           |
          +-----------+-----------+
                      |
                 Deduplicate
                      |
                    httpx
                      |
               Live Web Apps
                      |
          +-----------+-----------+
          |                       |
       Katana                 GAU / Wayback
          |                       |
          +-----------+-----------+
                      |
                   URLs
                      |
          +-----------+-----------+
          |                       |
        FFUF                 Feroxbuster
          |                       |
          +-----------+-----------+
                      |
                Attack Surface
                      |
                Nuclei Checks
                      |
                Burp / Caido
                      |
              Manual Validation
```

---

# Automation vs Manual Testing

Automation provides:

```text
Breadth
Speed
Coverage
```

Manual testing provides:

```text
Context
Logic
Validation
Impact analysis
```

Therefore:

```text
Automation
    +
Manual Analysis
    =
Better Assessment
```

---

# Important Rule

Never think:

```text
Tool Output
=
Confirmed Vulnerability
```

Instead:

```text
Tool Output
     ↓
Potential Finding
     ↓
Manual Validation
     ↓
Security Impact
     ↓
Confirmed Finding
```

---

# Scope

Every tool in this module must operate within:

```text
Authorized Scope
```

A tool may discover:

```text
Third-party systems
CDNs
Cloud infrastructure
SaaS providers
External authentication
Historical domains
```

Discovery does not automatically authorize testing.

---

# Operational Security

Consider:

```text
Request volume
Rate limits
Account lockouts
Application load
Network logs
Sensitive output
Credential storage
```

Different tools have very different operational impact.

---

# Tool Selection

Do not ask:

```text
Which tool is best?
```

Ask:

```text
What question am I trying to answer?
```

Examples:

```text
What subdomains exist?
        ↓
Subfinder / Amass

Which hosts are alive?
        ↓
httpx

What URLs exist?
        ↓
Katana / GAU / Waybackurls

What hidden paths exist?
        ↓
FFUF / Feroxbuster / Gobuster

How does this request behave?
        ↓
Burp / Caido

Does a known signature match?
        ↓
Nuclei

How strong are offline passwords?
        ↓
Hashcat / John

What is happening on the network?
        ↓
Tcpdump / Wireshark
```

---

# Key Takeaway

Tools should form a methodology:

```text
DISCOVER
   ↓
VALIDATE
   ↓
ENUMERATE
   ↓
ANALYZE
   ↓
TEST
   ↓
VERIFY
   ↓
DOCUMENT
```

Do not become dependent on individual commands.

Understand:

```text
Input
  ↓
Tool
  ↓
Output
  ↓
What does the output actually prove?
```

That distinction is more important than memorizing hundreds of flags.
