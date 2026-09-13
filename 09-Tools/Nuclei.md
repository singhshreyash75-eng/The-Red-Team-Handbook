# Nuclei

> ProjectDiscovery Nuclei fundamentals for template-based security checks in authorized environments.

## What is Nuclei?

**Nuclei** is a template-driven security scanner developed by ProjectDiscovery.

Instead of hardcoding every security check inside the scanner, Nuclei uses:

```text
Templates
```

that describe what to request and what response conditions indicate a potential finding.

Conceptually:

```text
Target
  +
Template
   ↓
Nuclei
   ↓
Request
   ↓
Response Matching
   ↓
Potential Finding
```

---

# Core Idea

Traditional scanner:

```text
Scanner
 |
Built-in checks
```

Nuclei:

```text
Nuclei Engine
     |
     +-- Template A
     +-- Template B
     +-- Template C
```

This makes checks modular and extensible.

---

# What Can Nuclei Check?

Depending on available templates, Nuclei can help identify conditions such as:

```text
Known CVEs
Misconfigurations
Exposed panels
Default files
Information disclosure
Technology fingerprints
Exposed services
Weak configurations
```

Not every template represents a vulnerability.

---

# Basic Usage

For a single authorized target:

```bash
nuclei -u https://example.test
```

For a target list:

```bash
nuclei -l live.txt
```

Always check:

```bash
nuclei -h
```

because syntax and template capabilities evolve.

---

# Templates

Nuclei templates are commonly written in:

```text
YAML
```

Conceptually:

```yaml
id: example-check

info:
  name: Example Check
  severity: info
```

Templates can define:

```text
Requests
Matchers
Extractors
Metadata
Severity
```

---

# Template Workflow

```text
Template
   |
Defines Request
   ↓
Nuclei Sends Request
   ↓
Target Responds
   ↓
Matchers Evaluate Response
   ↓
Finding / No Finding
```

---

# Matcher

A **matcher** determines whether a response satisfies particular conditions.

Examples conceptually include:

```text
Status code
Response word
Header
Regex
Binary condition
```

Example:

```text
Response contains:
"Example Dashboard"
```

This might indicate a particular product is exposed.

---

# Extractor

An **extractor** pulls useful information from a response.

Conceptually:

```text
Response
   ↓
Regex / JSON / Header extraction
   ↓
Version / Identifier / Value
```

Extraction is useful for enrichment and triage.

---

# Severity

Templates may use severity levels such as:

```text
info
low
medium
high
critical
```

Do not assume:

```text
Template says critical
       =
Confirmed critical vulnerability
```

Severity is only one part of validation.

---

# Nuclei Is Not Proof

This is the most important rule:

```text
Nuclei Match
     !=
Confirmed Vulnerability
```

Possible false-positive causes:

```text
Generic response text
Unexpected application behavior
Version mismatch
Reverse proxy
WAF response
Template limitation
Configuration differences
```

Always manually validate significant results.

---

# Templates and Trust

Templates can instruct the scanner to make network requests.

Therefore treat templates as:

```text
Code / Security Logic
```

Review templates from untrusted sources before running them.

Use trusted template repositories and understand potentially intrusive checks.

---

# Template Updates

ProjectDiscovery maintains a large community template ecosystem.

Template sets evolve rapidly because:

```text
New CVEs appear
Signatures improve
False positives are fixed
Checks are added
```

Keeping templates current improves coverage, but changes should still be reviewed in sensitive environments.

---

# Nuclei + httpx

One of the most common recon pipelines:

```text
Subfinder
   ↓
Subdomains
   ↓
httpx
   ↓
Live HTTP Services
   ↓
Nuclei
   ↓
Potential Findings
```

This avoids sending HTTP templates to obviously dead hosts.

---

# Nuclei + Katana

```text
Katana
 ↓
Endpoints
 ↓
Nuclei
 ↓
Template Checks
```

Endpoint-level scanning can provide more coverage than checking only application roots.

---

# Nuclei + GAU

Historical endpoints can also become candidates:

```text
GAU
 ↓
Historical URLs
 ↓
Validate Live
 ↓
Nuclei
```

Scope-check historical URLs before active scanning.

---

# Nuclei + Burp / Caido

The correct workflow for important findings:

```text
Nuclei
 ↓
Potential Finding
 ↓
Burp / Caido
 ↓
Manual Reproduction
 ↓
Confirmed / False Positive
```

Automation provides breadth.

Manual testing provides confidence.

---

# Technology-Based Scanning

Instead of blindly running every possible check:

```text
Fingerprint Technology
       ↓
Select Relevant Templates
       ↓
Run Checks
```

This reduces:

```text
Traffic
Noise
False positives
Runtime
```

---

# CVE Templates

Nuclei can contain checks for known vulnerabilities.

Conceptually:

```text
CVE
 ↓
Template
 ↓
Target Request
 ↓
Expected Vulnerable Behavior?
```

But:

```text
CVE Template Match
```

still requires verification against:

```text
Product
Version
Configuration
Patch Status
Actual Impact
```

---

# Version Detection Problem

Suppose a response appears to expose:

```text
Product X 1.0
```

A template may associate that with a CVE.

However:

```text
Backported patch
Reverse proxy
Modified product
Incorrect banner
```

can make version-only detection unreliable.

Always cross-check vendor advisories.

---

# Informational Templates

Not all results are vulnerabilities.

Examples:

```text
Technology detected
Login panel found
Service identified
Version disclosed
```

These may simply improve attack-surface understanding.

---

# Misconfiguration Checks

Nuclei can help identify potential configuration problems such as:

```text
Exposed administrative interfaces
Public debug endpoints
Unexpected files
Weak security configuration
```

Again, context determines whether a condition is actually risky.

---

# Rate Limiting

Nuclei can generate substantial request volume.

Consider:

```text
1000 Targets
×
100 Templates
=
Potentially huge request count
```

Control:

```text
Rate
Concurrency
Template selection
Target count
```

according to the rules of engagement.

---

# Intrusive Templates

Some security checks may:

```text
Trigger application behavior
Send unusual payloads
Create significant traffic
Interact with sensitive functionality
```

Do not assume every template is safe for every production environment.

Understand what you are running.

---

# Out-of-Band Checks

Some vulnerability classes may require an external interaction to confirm behavior.

Conceptually:

```text
Target
 ↓
Payload
 ↓
Target makes external request
 ↓
Confirmation
```

These checks require additional care because they involve infrastructure beyond a simple request/response flow.

Use only where explicitly authorized.

---

# False Positive Validation

For a significant match:

```text
1. Read template

2. Identify exact matcher

3. Reproduce request manually

4. Inspect complete response

5. Confirm product/version

6. Check vendor advisory

7. Determine actual impact

8. Record evidence
```

---

# False Negatives

No Nuclei result does not mean:

```text
Application is secure
```

Nuclei only checks conditions represented by:

```text
Templates
+
Reachable attack surface
+
Scanner logic
```

Business-logic vulnerabilities and many authorization flaws require manual testing.

---

# Nuclei vs Nessus-Type Scanners

Conceptually:

```text
Traditional Vulnerability Scanner
        |
Large integrated vulnerability engine
```

versus:

```text
Nuclei
   |
Template-driven checks
```

Nuclei's flexibility and community templates make it particularly useful in reconnaissance pipelines.

---

# Nuclei vs Burp

```text
Nuclei
 |
Automated breadth
```

```text
Burp
 |
Manual web-testing depth
```

A strong workflow uses both:

```text
Automation
   ↓
Interesting Finding
   ↓
Manual Validation
```

---

# Nuclei vs Nmap

```text
Nmap
 |
Network/service discovery
```

```text
Nuclei
 |
Template-driven application/service checks
```

They solve different layers of the assessment.

---

# Recommended Pipeline

```text
Amass / Subfinder
       ↓
Subdomains
       ↓
httpx
       ↓
Live Services
       ↓
Katana / GAU
       ↓
Endpoints
       ↓
Nuclei
       ↓
Potential Findings
       ↓
Burp / Caido
       ↓
Manual Validation
```

---

# Defensive Use

Defenders can also use Nuclei for:

```text
Attack-surface monitoring
Exposure validation
Known-CVE checks
Configuration auditing
Continuous security testing
```

The same principle applies:

```text
Scanner Finding
      ↓
Validate
      ↓
Remediate
      ↓
Retest
```

---

# Reporting

Do not report only:

```text
"Nuclei found CVE-X."
```

A stronger finding includes:

```text
Affected asset
Exact endpoint
Observed behavior
Product/version
Relevant CVE/configuration
Manual validation
Security impact
Remediation
```

---

# Key Takeaway

Nuclei's core architecture is:

```text
TARGET
  +
TEMPLATE
   ↓
REQUEST
   ↓
MATCHER
   ↓
POTENTIAL FINDING
```

The final step must be:

```text
Potential Finding
       ↓
MANUAL VALIDATION
       ↓
Confirmed Vulnerability
       or
False Positive
```

Nuclei is excellent for **scalable breadth**, but it does not replace manual security analysis.
