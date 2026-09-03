# CVSS

> Common Vulnerability Scoring System fundamentals for understanding vulnerability severity.

## What is CVSS?

**CVSS = Common Vulnerability Scoring System**

CVSS provides a standardized method for describing and scoring vulnerability severity.

Typical scores range from:

```text
0.0 → 10.0
```

Example:

```text
CVSS Score: 9.8
```

---

# Severity Ratings

Common CVSS v3.x qualitative ranges:

```text
0.0          None

0.1 - 3.9    Low

4.0 - 6.9    Medium

7.0 - 8.9    High

9.0 - 10.0   Critical
```

---

# Important Warning

```text
CVSS != Risk
```

CVSS measures standardized technical severity.

Real organizational risk also depends on:

```text
Asset importance
Internet exposure
Exploit availability
Threat activity
Business impact
Compensating controls
Environment
```

Example:

```text
CVSS 9.8 vulnerability
on isolated unused test system
```

may have lower operational priority than:

```text
CVSS 8.1 vulnerability
on internet-facing production identity server
```

---

# CVSS Vector

A CVSS score is accompanied by a vector.

Example:

```text
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H
```

Instead of memorizing the number alone, understand the vector.

---

# Base Metrics

CVSS v3.1 base metrics include:

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

---

# Attack Vector — AV

Describes how remotely the vulnerability can be exploited.

Values:

```text
N = Network
A = Adjacent
L = Local
P = Physical
```

### Network

```text
AV:N
```

Attacker can potentially reach the vulnerable component over a network.

### Adjacent

```text
AV:A
```

Attack requires an adjacent network relationship.

### Local

```text
AV:L
```

Requires local system access.

### Physical

```text
AV:P
```

Requires physical interaction.

---

# Attack Complexity — AC

```text
AC:L = Low
AC:H = High
```

Low:

```text
Few special conditions
```

High:

```text
Special conditions/timing/environment required
```

Generally:

```text
AC:L
```

contributes to greater severity.

---

# Privileges Required — PR

Values:

```text
PR:N = None
PR:L = Low
PR:H = High
```

Example:

```text
PR:N
```

means exploitation does not require prior privileges.

---

# User Interaction — UI

CVSS v3.1 commonly uses:

```text
UI:N = None
UI:R = Required
```

Example requiring:

```text
Victim opens malicious file
```

could involve:

```text
UI:R
```

---

# Scope — S

Values:

```text
S:U = Unchanged
S:C = Changed
```

Scope asks whether exploitation can affect security authority beyond the vulnerable component's original security scope.

This is one of the more subtle CVSS concepts.

---

# Confidentiality — C

Impact values:

```text
C:N = None
C:L = Low
C:H = High
```

High confidentiality impact could involve major disclosure of protected information.

---

# Integrity — I

```text
I:N
I:L
I:H
```

High integrity impact means substantial ability to modify protected data or system state.

---

# Availability — A

```text
A:N
A:L
A:H
```

High availability impact may involve severe disruption of the affected component.

---

# Example Vector

Consider:

```text
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H
```

Breakdown:

```text
AV:N
Remote/network reachable

AC:L
Low attack complexity

PR:N
No prior privileges

UI:N
No user interaction

S:U
Scope unchanged

C:H
High confidentiality impact

I:H
High integrity impact

A:H
High availability impact
```

This combination results in a very high base score.

---

# Why RCE Often Scores Highly

A remote vulnerability may have:

```text
AV:N
AC:L
PR:N
UI:N
```

and potentially:

```text
C:H
I:H
A:H
```

This combination represents a particularly severe technical condition.

But:

```text
RCE != automatically CVSS 10
```

The actual vector determines the score.

---

# CVSS Metric Groups

CVSS v3.x includes:

```text
Base Metrics
Temporal Metrics
Environmental Metrics
```

### Base

Intrinsic characteristics of the vulnerability.

### Temporal

Factors that may change over time.

### Environmental

Adjusts severity based on a particular environment.

---

# CVSS v4

Modern vulnerability records may use:

```text
CVSS v4.0
```

CVSS v4 introduces an updated model and terminology beyond CVSS v3.1.

When reading a score, always check:

```text
Which CVSS version?
```

Do not compare vectors without noticing version differences.

---

# CVSS vs CVE

```text
CVE
 =
Which vulnerability?
```

```text
CVSS
 =
How technically severe is it?
```

Example:

```text
CVE-XXXX-YYYY
      |
      +--> CVSS score/vector
```

---

# CVSS vs CWE

Another important distinction:

```text
CVE
 |
Specific vulnerability
```

```text
CWE
 |
Weakness category
```

```text
CVSS
 |
Severity scoring
```

Example concept:

```text
CWE
SQL Injection
     ↓
Specific product vulnerability
     ↓
CVE
     ↓
Severity
     ↓
CVSS
```

---

# CVSS vs EPSS

You may also encounter:

```text
EPSS
=
Exploit Prediction Scoring System
```

CVSS asks roughly:

```text
How severe is the vulnerability?
```

EPSS estimates:

```text
How likely is exploitation in the wild
within its prediction framework?
```

These metrics answer different questions.

---

# Prioritization

A better vulnerability-management model combines:

```text
CVSS
  +
Exploitability
  +
Threat intelligence
  +
Asset criticality
  +
Exposure
  +
Business impact
```

Example:

```text
CVE A
CVSS 9.8
Internal isolated test server

CVE B
CVSS 8.8
Internet-facing production server
Active exploitation observed
```

CVE B may deserve faster remediation.

---

# Common Mistakes

Do not assume:

```text
CVSS 10 = definitely exploitable
```

or:

```text
CVSS 5 = harmless
```

or:

```text
Higher CVSS always means
higher organizational risk
```

Context matters.

---

# Vulnerability Review Workflow

```text
CVE
 ↓
Read CVSS Vector
 ↓
Understand Attack Requirements
 ↓
Understand Technical Impact
 ↓
Check Exploitability
 ↓
Check Asset Exposure
 ↓
Check Business Importance
 ↓
Prioritize
```

---

# Quick Cheat Sheet

```text
AV = How attacker reaches it

AC = How difficult exploitation is

PR = Existing privilege required

UI = Victim interaction required

S  = Security scope change

C  = Confidentiality impact

I  = Integrity impact

A  = Availability impact
```

---

# Key Takeaway

Don't memorize only:

```text
CVSS = 9.8
```

Read:

```text
AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H
```

because the vector explains **why** the vulnerability received its severity.

And always remember:

```text
CVSS
   =
Technical Severity

CVSS
   !=
Complete Organizational Risk
```
