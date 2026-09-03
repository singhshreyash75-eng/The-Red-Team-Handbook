# MITRE

> MITRE CVE, CWE, and ATT&CK fundamentals and how they relate to vulnerability and threat research.

## What is MITRE?

**MITRE** is a nonprofit organization involved in multiple cybersecurity programs and knowledge bases.

In security research, you frequently encounter MITRE in connection with:

```text
CVE
CWE
ATT&CK
CAPEC
```

These systems answer different questions.

---

# The Big Picture

```text
CWE
 |
What TYPE of weakness?
 |
 v
CVE
 |
Which SPECIFIC vulnerability?
```

Separately:

```text
MITRE ATT&CK
 |
How do adversaries behave?
```

And:

```text
CAPEC
 |
Attack-pattern knowledge
```

Do not treat these as interchangeable databases.

---

# CVE

**CVE = Common Vulnerabilities and Exposures**

CVE provides standardized identifiers for publicly disclosed vulnerabilities.

Example:

```text
CVE-2021-44228
```

CVE answers:

```text
Which specific vulnerability
are we talking about?
```

---

# CVE Record

A CVE record may include:

```text
CVE ID
Description
Affected product information
References
CNA information
Problem types
```

A CVE record is the standardized identity/reference point for the vulnerability.

---

# CNA

**CNA = CVE Numbering Authority**

CNAs can assign CVE IDs within their defined scope.

Examples may include:

```text
Vendors
CERT organizations
Security organizations
Research organizations
```

Conceptually:

```text
Vulnerability
     ↓
CNA
     ↓
CVE ID
```

---

# CVE States

You may encounter states such as:

```text
RESERVED
PUBLISHED
REJECTED
```

## Reserved

An ID exists, but public details may not yet be available.

## Published

The CVE record is publicly available.

## Rejected

The CVE ID should no longer be treated as a valid independent vulnerability record.

Always check current status.

---

# CWE

**CWE = Common Weakness Enumeration**

CWE describes **classes of software/hardware weaknesses**.

Example concepts:

```text
SQL Injection
Buffer Overflow
Path Traversal
Command Injection
Improper Access Control
```

A CWE is generally broader than a CVE.

---

# CVE vs CWE

Example:

```text
CWE
 |
SQL Injection
 |
 +--> CVE-A
 +--> CVE-B
 +--> CVE-C
```

Therefore:

```text
CWE
=
Weakness category
```

while:

```text
CVE
=
Specific vulnerability instance
```

---

# Why CWE Matters

If you only memorize CVEs:

```text
CVE-1
CVE-2
CVE-3
```

you learn individual incidents.

If you understand CWE categories:

```text
Input Validation
Memory Safety
Access Control
Authentication
```

you learn recurring vulnerability patterns.

---

# Common Weakness Categories

Examples include:

```text
Injection
Memory corruption
Improper authentication
Improper authorization
Path traversal
Information exposure
Race conditions
Cryptographic weaknesses
Unsafe deserialization
```

---

# MITRE ATT&CK

**ATT&CK = Adversarial Tactics, Techniques, and Common Knowledge**

ATT&CK describes adversary behaviors observed in real-world environments.

Conceptually:

```text
Adversary Objective
       ↓
Tactic
       ↓
Technique
       ↓
Sub-technique
```

---

# Tactic

A **tactic** represents:

```text
WHY
```

an adversary performs an action.

Examples include:

```text
Initial Access
Execution
Persistence
Privilege Escalation
Defense Evasion
Credential Access
Discovery
Lateral Movement
Collection
Command and Control
Exfiltration
Impact
```

---

# Technique

A **technique** describes:

```text
HOW
```

an adversary may achieve an objective.

Example conceptual relationship:

```text
Credential Access
       |
       +--> Credential Dumping
```

---

# Sub-Technique

Some techniques contain more specific:

```text
Sub-techniques
```

Conceptually:

```text
Technique
   |
   +-- Specific method A
   +-- Specific method B
```

This allows ATT&CK to represent behavior at greater granularity.

---

# Tactic vs Technique

Remember:

```text
TACTIC
=
Why?
```

```text
TECHNIQUE
=
How?
```

Example:

```text
Why?
Credential Access

How?
Credential Dumping
```

---

# ATT&CK Matrix

ATT&CK is often visualized as a matrix:

```text
Initial     Execution     Persistence     Priv Esc
Access

  |             |              |             |
Techniques   Techniques     Techniques    Techniques
```

This helps map adversary behavior across an intrusion lifecycle.

---

# CVE vs ATT&CK

Very important distinction:

```text
CVE
=
Specific software vulnerability
```

```text
ATT&CK
=
Adversary behavior
```

Example conceptual chain:

```text
CVE exploited
      ↓
Initial Access
      ↓
Execution
      ↓
Credential Access
      ↓
Lateral Movement
```

Only the first step may depend on a CVE.

The rest can involve legitimate system functionality.

---

# Why ATT&CK Matters to Red Teams

During authorized adversary simulation, ATT&CK can help structure:

```text
Objectives
Techniques
Coverage
Reporting
Detection testing
```

Instead of reporting only:

```text
"We ran tool X"
```

report:

```text
Behavior
   ↓
ATT&CK Technique
   ↓
Observed detection/control
```

This makes results more transferable across tools.

---

# Why ATT&CK Matters to Blue Teams

Defenders can map:

```text
Telemetry
Detections
Security Controls
Incidents
```

to ATT&CK techniques.

Example:

```text
Technique
   ↓
Required telemetry
   ↓
Detection rule
```

This helps identify detection gaps.

---

# Tool vs Technique

A critical concept:

```text
Tool != Technique
```

Many tools can implement the same technique.

Example:

```text
Tool A ----+
Tool B ----+--> Credential Dumping
Tool C ----+
```

If defenders detect only:

```text
Tool A filename
```

another implementation may bypass that detection.

Behavior-based detection is stronger.

---

# CAPEC

**CAPEC = Common Attack Pattern Enumeration and Classification**

CAPEC catalogs attack patterns.

Conceptually:

```text
CWE
 |
Weakness
```

```text
CAPEC
 |
Attack pattern targeting weakness
```

```text
CVE
 |
Specific vulnerability
```

These knowledge bases can be linked conceptually.

---

# CVE + CWE + CAPEC

Example model:

```text
CWE
 |
Weakness Type
 |
 v
CAPEC
 |
Attack Pattern
 |
 v
CVE
 |
Specific Product Vulnerability
```

Relationships are not always one-to-one.

---

# MITRE Research Workflow

When researching a vulnerability:

```text
CVE
 ↓
Read CVE Record
 ↓
Identify CWE
 ↓
Understand weakness class
 ↓
Read vendor advisory
 ↓
Assess impact
```

When analyzing adversary behavior:

```text
Observed Behavior
      ↓
ATT&CK Technique
      ↓
Tactic
      ↓
Detection / Mitigation
```

---

# Example Security Report Mapping

Instead of:

```text
Attacker dumped credentials.
```

a report may include:

```text
Finding:
Credential material accessed

ATT&CK:
Credential Access

Technique:
Relevant credential-access technique

Evidence:
Endpoint telemetry

Mitigation:
Credential protection
```

Exact ATT&CK IDs should be verified against the current ATT&CK catalog rather than guessed.

---

# ATT&CK Changes Over Time

MITRE ATT&CK evolves.

Techniques can be:

```text
Added
Updated
Deprecated
Reorganized
```

Therefore always consult the current ATT&CK documentation when producing formal mappings.

---

# MITRE Does Not Equal NVD

Another important distinction:

```text
CVE Program
```

and:

```text
NVD
```

are related but different.

Conceptually:

```text
CVE
 |
Standard vulnerability record
 |
 v
NVD
 |
Additional analysis/enrichment
```

NVD is maintained by NIST, not MITRE.

---

# MITRE vs NIST

Simplified:

```text
MITRE / CVE Program
       ↓
CVE identification ecosystem
```

```text
NIST
       ↓
NVD vulnerability enrichment
```

You will often use both during research.

---

# Security Knowledge Map

```text
              SECURITY RESEARCH
                     |
        +------------+------------+
        |                         |
 Vulnerabilities              Adversaries
        |                         |
   CVE / CWE                  ATT&CK
        |                         |
 Specific bugs              Behaviors
        |
     CAPEC
        |
 Attack patterns
```

---

# Key Takeaway

Memorize the distinction:

```text
CVE
=
Specific vulnerability
```

```text
CWE
=
Weakness category
```

```text
CAPEC
=
Attack pattern
```

```text
ATT&CK
=
Adversary behavior
```

Together they help answer:

```text
What weakness exists?
Which vulnerability represents it?
How could it be attacked?
How do real adversaries behave?
How should defenders detect it?
```
