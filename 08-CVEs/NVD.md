# NVD

> National Vulnerability Database fundamentals, CVE enrichment, CVSS, CPE, CWE, and vulnerability research workflow.

## What is NVD?

**NVD = National Vulnerability Database**

The NVD is maintained by:

```text
NIST
=
National Institute of Standards and Technology
```

It provides vulnerability information and enrichment based around CVE records.

Conceptually:

```text
CVE Record
    ↓
NVD Analysis / Enrichment
    ↓
CVSS
CWE
CPE
References
Affected configurations
```

---

# NVD vs CVE

Do not treat these as the same thing.

```text
CVE
=
Standardized vulnerability identifier/record
```

```text
NVD
=
Vulnerability database that enriches
CVE-related information
```

Conceptually:

```text
CVE-XXXX-YYYY
      ↓
NVD Entry
      |
      +-- CVSS
      +-- CWE
      +-- CPE
      +-- References
```

---

# NVD vs MITRE

Simplified:

```text
CVE Program
   |
CVE record
```

then:

```text
NVD
 |
Additional vulnerability analysis
and metadata
```

NVD is operated by:

```text
NIST
```

not MITRE.

---

# What Does an NVD Entry Contain?

Information may include:

```text
CVE ID
Description
CVSS scores
CVSS vectors
CWE
CPE/configuration data
References
Publication dates
Modification dates
Vendor information
```

The exact information varies by vulnerability and processing state.

---

# CVSS

NVD commonly displays:

```text
CVSS
```

scores and vectors.

Example:

```text
9.8 CRITICAL
```

with a vector such as:

```text
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H
```

Never read only the number.

Read the vector.

---

# CVSS Components

Common CVSS v3.1 metrics:

```text
AV = Attack Vector
AC = Attack Complexity
PR = Privileges Required
UI = User Interaction
S  = Scope
C  = Confidentiality
I  = Integrity
A  = Availability
```

Example:

```text
AV:N
```

means network attack vector.

---

# CVSS Versions

You may encounter:

```text
CVSS v2
CVSS v3.0
CVSS v3.1
CVSS v4.0
```

Always check:

```text
Which version produced this score?
```

Scores/vectors from different CVSS versions should not be interpreted as identical models.

---

# CWE

NVD entries may map vulnerabilities to:

```text
CWE
=
Common Weakness Enumeration
```

Example concept:

```text
CVE
 |
Specific vulnerability
 |
Mapped to
 |
CWE
 |
Weakness category
```

This helps understand the underlying bug class.

---

# CPE

**CPE = Common Platform Enumeration**

CPE provides standardized names for products/platforms.

Conceptually:

```text
Vendor
Product
Version
Platform
```

can be represented in a machine-readable standardized format.

---

# Why CPE Matters

Vulnerability-management systems need to determine:

```text
Which products
are affected by
which CVEs?
```

CPE helps map vulnerability information to products.

Conceptually:

```text
Asset Inventory
      |
Product / Version
      |
      v
CPE
      |
      v
Relevant CVEs
```

---

# CPE Structure

A CPE string can encode information such as:

```text
Part
Vendor
Product
Version
Update
Edition
Language
```

You do not need to memorize every field initially.

Understand its purpose:

```text
Standardized product identification
```

---

# Affected Configurations

NVD may provide configuration information indicating affected products and versions.

Example concept:

```text
Product X
 |
Versions >= 1.0
and < 1.5
```

This is more useful than simply seeing:

```text
Product X vulnerable
```

---

# Version Ranges

Always inspect exact ranges.

Example:

```text
Affected:
>= 2.0
< 2.5.7
```

Target:

```text
2.5.6
```

may be affected.

Target:

```text
2.5.7
```

may be fixed.

But always confirm with:

```text
Vendor advisory
```

because NVD metadata may be incomplete or updated over time.

---

# Vendor Advisory

For definitive remediation information, prioritize the vendor's security advisory when available.

Good research:

```text
NVD
  +
CVE Record
  +
Vendor Advisory
  +
Patch Notes
```

NVD should not be your only source.

---

# References

NVD entries often link to references such as:

```text
Vendor advisories
Patch commits
Mailing lists
Security research
CERT advisories
GitHub advisories
```

References are often where the most useful technical detail exists.

---

# Published vs Modified Date

NVD records commonly include:

```text
Published
Last Modified
```

The modified date matters because vulnerability information can change.

Examples:

```text
Affected versions updated
CVSS changed
References added
CPE data corrected
```

Therefore vulnerability intelligence is not always static.

---

# CVSS Source

You may encounter more than one CVSS assessment.

For example:

```text
NVD assessment
Vendor/CNA assessment
```

Different organizations may assign different vectors based on different interpretations or information.

Do not automatically assume one number tells the entire story.

---

# Severity vs Exploitation

A high CVSS score does not prove:

```text
Exploit exists
```

Similarly, a medium/high vulnerability can become operationally urgent when:

```text
Reliable exploit exists
+
Internet exposure
+
Active exploitation
```

---

# NVD vs Exploit-DB

```text
NVD
 |
Vulnerability information
```

versus:

```text
Exploit-DB
 |
Public exploit / PoC research
```

Workflow:

```text
NVD
 ↓
Understand vulnerability
 ↓
Exploit-DB / research
 ↓
Determine public exploitability
```

---

# NVD vs Searchsploit

```text
NVD
=
CVE metadata and enrichment
```

```text
Searchsploit
=
Local search interface for
Exploit-DB material
```

They solve different problems.

---

# NVD Research Workflow

Suppose you discover:

```text
Apache / Product X
Version Y
```

Use:

```text
Product
   ↓
Exact Version
   ↓
Search NVD/CVE
   ↓
Potential CVEs
   ↓
Read affected configurations
   ↓
Read CVSS vector
   ↓
Read CWE
   ↓
Read references
   ↓
Vendor advisory
   ↓
Confirm applicability
```

---

# Scanner Results

A vulnerability scanner may identify:

```text
CVE-XXXX-YYYY
```

based on product/version fingerprints.

Do not automatically mark:

```text
Confirmed
```

Instead:

```text
Scanner Finding
      ↓
NVD Research
      ↓
Vendor Advisory
      ↓
Patch / Version Verification
      ↓
Manual Validation
```

---

# Backported Patches

This is especially important on Linux.

Example:

```text
Package version appears old
        ↓
Distribution vendor applied
security patch to old branch
        ↓
Version string still appears old
```

Therefore:

```text
NVD version comparison
```

alone may generate false positives.

Check:

```text
Distribution security advisory
Package changelog
Vendor patch status
```

---

# CISA KEV

For prioritization, another useful source is:

```text
CISA KEV
=
Known Exploited Vulnerabilities Catalog
```

It identifies vulnerabilities known to have been exploited in the wild according to CISA's catalog criteria.

Conceptually:

```text
NVD
 |
Severity / vulnerability data
```

plus:

```text
CISA KEV
 |
Known exploitation context
```

can improve prioritization.

---

# EPSS

Another useful metric:

```text
EPSS
=
Exploit Prediction Scoring System
```

Conceptually:

```text
CVSS
 |
Technical severity
```

versus:

```text
EPSS
 |
Probability-oriented exploitation estimate
```

and:

```text
CISA KEV
 |
Known exploitation evidence/catalog status
```

These answer different questions.

---

# Better Prioritization

Do not prioritize only by:

```text
CVSS
```

Use:

```text
CVSS
   +
Internet Exposure
   +
Asset Criticality
   +
Exploit Availability
   +
CISA KEV
   +
Threat Intelligence
   +
Business Impact
```

---

# Example

Suppose:

```text
CVE-A
CVSS: 9.8
No known exploitation
Internal test server
```

and:

```text
CVE-B
CVSS: 8.1
Known exploitation
Internet-facing production system
```

Operationally:

```text
CVE-B
```

may deserve more immediate remediation.

---

# Vulnerability Research Checklist

For every NVD entry, answer:

```text
CVE ID?

Affected product?

Affected versions?

CVSS version?

CVSS vector?

CWE?

CPE?

Remote or local?

Authentication required?

User interaction required?

Vendor advisory?

Patch available?

Public exploit?

Known exploitation?

Mitigations?
```

---

# Research Sources

A strong vulnerability investigation may combine:

```text
CVE Program
      +
NVD
      +
Vendor Advisory
      +
CISA
      +
CERT
      +
Exploit-DB
      +
Technical Research
```

No single database tells the entire story.

---

# Vulnerability Management Flow

```text
Asset Inventory
      ↓
Product / Version
      ↓
CPE / Vulnerability Mapping
      ↓
CVE
      ↓
NVD / Vendor Analysis
      ↓
Severity + Exploitability
      ↓
Prioritization
      ↓
Patch / Mitigation
      ↓
Verification
```

---

# Key Takeaway

Remember:

```text
CVE
=
Which vulnerability?
```

```text
NVD
=
What additional standardized
information do we know about it?
```

```text
CVSS
=
How technically severe?
```

```text
CWE
=
What weakness class?
```

```text
CPE
=
Which product/platform?
```

And never use:

```text
CVSS alone
```

as your complete vulnerability-prioritization strategy.

Use:

```text
Severity
   +
Exploitability
   +
Exposure
   +
Asset Importance
   +
Threat Activity
   =
Real Remediation Priority
```
