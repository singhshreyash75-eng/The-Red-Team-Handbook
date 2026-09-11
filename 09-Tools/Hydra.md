# Hydra

> THC Hydra fundamentals for controlled online authentication auditing in explicitly authorized environments.

## What is Hydra?

**Hydra**, commonly called **THC Hydra**, is an online authentication-testing tool.

It can automate credential attempts against supported network authentication protocols.

Conceptually:

```text
Username Candidates
        +
Password Candidates
        ↓
      Hydra
        ↓
Authentication Service
        ↓
Success / Failure
```

Its key characteristic is:

```text
ONLINE authentication testing
```

---

# Hydra vs Hashcat

This distinction is critical.

## Hydra

```text
Candidate Credential
        ↓
Remote Login Service
        ↓
Success / Failure
```

Therefore:

```text
ONLINE
```

## Hashcat

```text
Hash
  ↓
Local Candidate Calculation
  ↓
Comparison
```

Therefore:

```text
OFFLINE
```

Remember:

```text
Hydra   = Online authentication audit

Hashcat = Offline password/hash audit
```

---

# Why Online Testing is Different

Every Hydra attempt may reach the actual authentication service.

Therefore it can trigger:

```text
Authentication logs
Account lockouts
Rate limiting
MFA challenges
IDS/IPS alerts
WAF controls
Temporary blocking
```

This makes online password auditing significantly more operationally sensitive than offline hash analysis.

---

# Authorization Requirement

Credential guessing can disrupt accounts and services.

Only perform it when the rules of engagement explicitly permit:

```text
Credential testing
Account lockout risk
Target protocol
Target accounts
Testing window
Rate limits
```

Never assume generic pentest authorization automatically includes unrestricted password spraying/brute force.

---

# Basic Model

Hydra requires information such as:

```text
Target
Protocol
Username or user list
Password or password list
```

Conceptually:

```text
users.txt
    +
passwords.txt
    +
Service
    ↓
Controlled Authentication Audit
```

---

# Supported Protocols

Hydra versions support many authentication mechanisms.

Commonly encountered categories include:

```text
SSH
FTP
HTTP authentication
SMTP
POP3
IMAP
Database services
Other network logins
```

Exact support depends on the installed version.

Check:

```bash
hydra -h
```

and module-specific help/documentation.

---

# Authentication Enumeration First

Before any credential testing, determine:

```text
Which service?
Which authentication mechanism?
Does MFA exist?
Does lockout exist?
Is rate limiting present?
Which test accounts are authorized?
```

Do not start with password guessing.

---

# Single Account vs Multiple Accounts

Two different concepts:

```text
One account
+
Many passwords
```

versus:

```text
Many accounts
+
Small controlled password set
```

Both can cause lockouts depending on policy.

The assessment plan should explicitly define the permitted strategy.

---

# Password Spraying

**Password spraying** conceptually tests a small number of approved candidate passwords across multiple accounts.

```text
Password Candidate
        |
        +--> User A
        +--> User B
        +--> User C
```

This is often discussed as an alternative to rapidly trying many passwords against one account.

However:

```text
Password spraying can still
cause lockouts and alerts.
```

It requires explicit authorization and careful rate control.

---

# Brute Force

A brute-force approach attempts many candidate combinations.

Conceptually:

```text
User
 |
 +-- Candidate 1
 +-- Candidate 2
 +-- Candidate 3
 +-- ...
```

Against real online services this can be:

```text
Noisy
Slow
Disruptive
Easily detected
```

and may trigger defensive controls.

---

# Account Lockout

Organizations may configure policies such as:

```text
5 failed attempts
      ↓
Account locked
```

Therefore before testing:

```text
Understand lockout policy
        ↓
Choose safe test strategy
```

If lockout behavior is unknown, coordinate with the system owner rather than guessing aggressively.

---

# Rate Limiting

Applications may enforce:

```text
Requests per minute
Progressive delays
Temporary IP blocks
CAPTCHA
Account throttling
```

These controls make high-speed online guessing inappropriate.

---

# MFA

If MFA is enabled:

```text
Password
   +
Second Factor
```

A correct password alone may not provide successful authentication.

This is an important defensive advantage.

---

# HTTP Authentication

Web authentication is not one single protocol.

Examples include:

```text
HTTP Basic
HTTP Digest
HTML login forms
API authentication
SSO
OAuth/OIDC
```

A generic HTTP password-testing tool must understand the actual authentication flow.

---

# Login Forms

Modern login forms may involve:

```text
CSRF tokens
Dynamic cookies
JavaScript
CAPTCHA
MFA
SSO
Anti-automation controls
```

Therefore simple automated credential submission may not accurately model the login flow.

Use Burp/Caido first to understand the request sequence.

---

# Hydra + Burp / Caido

For an authorized test application:

```text
Browser
   ↓
Burp / Caido
   ↓
Understand Login Flow
   ↓
Determine whether automated
testing is appropriate
```

Do not blindly point Hydra at a login page.

---

# Username Enumeration

Authentication responses sometimes reveal whether a username exists.

Example:

```text
Unknown user
```

versus:

```text
Incorrect password
```

This can create an account-enumeration weakness.

A secure application generally avoids unnecessarily revealing account existence.

---

# Response Differences

When auditing authentication, inspect:

```text
Status code
Response body
Redirect
Cookie creation
Response length
Timing
```

Do not rely on one indicator alone.

---

# Credential Reuse

One security objective of authorized authentication testing can be evaluating resistance to weak/reused credentials.

Conceptually:

```text
Weak Password
     +
Exposed Service
     ↓
Account Compromise
```

Defenses include:

```text
MFA
Strong password policy
Breach-password screening
Rate limiting
Lockout/throttling
Monitoring
```

---

# Default Credentials

Some products historically ship with or are deployed using default credentials.

Security reviews should determine whether default credentials have been changed.

However, testing should use approved credentials/candidates and avoid broad uncontrolled guessing.

---

# Wordlists

For controlled lab work, password candidate files may contain:

```text
Lab passwords
Approved test candidates
Generated policy-testing values
```

Do not use leaked credential datasets against real users without explicit authorization and appropriate data-handling procedures.

---

# Online Testing Efficiency

Unlike offline cracking:

```text
More attempts
```

is usually not the goal.

A better online security test asks:

```text
Does the system resist
a small, realistic,
authorized set of attempts?
```

Measure controls such as:

```text
MFA
Throttling
Lockout
Alerting
Detection
```

---

# Detection

Online credential attacks can generate:

```text
Failed authentication events
Multiple accounts from one source
Repeated attempts
Geographic anomalies
IP reputation alerts
Account lockouts
```

Defenders should correlate:

```text
Identity logs
Network telemetry
Application logs
SIEM alerts
```

---

# Defensive Controls

Use:

```text
MFA
Password managers
Strong unique passwords
Breach-password screening
Rate limiting
Progressive delays
Risk-based authentication
Account lockout where appropriate
Monitoring
SSO
```

---

# Hydra vs John the Ripper

```text
Hydra
 |
Remote authentication service
 |
ONLINE
```

```text
John the Ripper
 |
Local hash/password material
 |
OFFLINE
```

---

# Hydra vs Hashcat

Same key distinction:

```text
Hydra
   =
Online Login Testing
```

```text
Hashcat
   =
Offline Hash Auditing
```

---

# Safe Assessment Workflow

```text
1. Confirm authorization

2. Identify authentication protocol

3. Identify approved test accounts

4. Understand lockout policy

5. Understand MFA

6. Define rate limits

7. Use minimal candidate set

8. Monitor account/service health

9. Stop on unexpected behavior

10. Document defensive response
```

---

# Reporting

A useful finding should explain:

```text
Affected authentication service
Testing conditions
Rate/lockout controls
MFA status
Observed behavior
Security impact
Recommended remediation
```

Avoid placing real plaintext credentials unnecessarily into reports.

---

# Key Takeaway

Hydra's core model is:

```text
CREDENTIAL CANDIDATE
        ↓
REMOTE AUTHENTICATION
        ↓
SUCCESS / FAILURE
```

This makes it fundamentally different from:

```text
Hashcat
John the Ripper
```

which primarily operate on offline password material.

Because Hydra interacts with live authentication systems:

```text
AUTHORIZATION
   +
RATE CONTROL
   +
LOCKOUT AWARENESS
```

are essential.

The security goal is not maximum guessing speed—it is evaluating whether authentication controls resist realistic credential attacks.
