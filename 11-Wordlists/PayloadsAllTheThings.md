# PayloadsAllTheThings

> PayloadsAllTheThings fundamentals as a security-testing reference for authorized web application assessments and labs.

## What is PayloadsAllTheThings?

**PayloadsAllTheThings** is a large community-maintained repository containing security-testing references, techniques, examples, and payload patterns for many vulnerability classes.

It is commonly used by:

```text
Pentesters
Red Teamers
Bug Bounty Researchers
Application Security Engineers
CTF Players
Security Students
```

Think of it primarily as:

```text
Security Testing Reference
        +
Payload Collection
        +
Technique Documentation
```

---

# Is it a Traditional Wordlist?

Not exactly.

Unlike a simple directory wordlist:

```text
admin
login
backup
api
```

PayloadsAllTheThings is organized around:

```text
Vulnerability Classes
Testing Techniques
Payload Patterns
Bypass Concepts
References
```

Therefore:

```text
SecLists
     ↓
Mostly candidate lists
```

while:

```text
PayloadsAllTheThings
     ↓
Testing knowledge + payload references
```

---

# Common Topic Categories

The repository covers areas such as:

```text
SQL Injection
Cross-Site Scripting
Command Injection
Path Traversal
Server-Side Request Forgery
Server-Side Template Injection
XXE
File Upload
Open Redirect
LDAP Injection
NoSQL Injection
Authentication topics
Deserialization
JWT
GraphQL
Race Conditions
```

Exact repository structure evolves over time.

---

# What is a Payload?

A **payload** is test input designed to trigger or investigate specific application behavior.

Conceptually:

```text
Input
  ↓
Application Parser / Logic
  ↓
Unexpected Behavior?
```

Payloads are used to answer:

```text
"Does this input reach
a vulnerable code path?"
```

---

# Payload != Exploit

Important distinction:

```text
Payload
=
Test input
```

```text
Exploit
=
Technique/code that reliably
leverages a vulnerability
```

A payload producing an error is not automatically proof of exploitability.

---

# Testing Workflow

Correct workflow:

```text
Understand Input
      ↓
Identify Technology / Context
      ↓
Choose Relevant Test Pattern
      ↓
Send Controlled Input
      ↓
Observe Response
      ↓
Validate Vulnerability
      ↓
Determine Impact
```

Do not blindly paste hundreds of payloads into every parameter.

---

# Context Matters

Suppose input appears inside:

```text
HTML
JavaScript
SQL query
Shell command
Template
URL
JSON
XML
```

The appropriate testing strategy changes completely.

Therefore:

```text
Same Payload
     +
Different Context
     =
Different Result
```

---

# Burp Suite Integration

A common workflow:

```text
Application
    ↓
Burp Proxy
    ↓
Interesting Parameter
    ↓
Repeater
    ↓
Relevant Payload Pattern
    ↓
Observe Response
```

Repeater is ideal because individual requests can be modified carefully.

---

# Caido Integration

Similarly:

```text
Capture
 ↓
Replay
 ↓
Modify Input
 ↓
Compare Response
```

PayloadsAllTheThings can serve as a reference while manually testing in Caido.

---

# XSS Reference

For Cross-Site Scripting, testing depends heavily on where user-controlled data appears.

Possible contexts include:

```text
HTML body
HTML attribute
JavaScript
URL
DOM
```

The core question is:

```text
Can attacker-controlled data
be interpreted as executable
browser content?
```

Use harmless proof-of-concept markers in authorized environments.

---

# SQL Injection Reference

SQL injection occurs when untrusted input influences a database query unsafely.

Conceptually:

```text
User Input
     ↓
Unsafe Query Construction
     ↓
Database Parser
     ↓
Unexpected Query Logic
```

Secure approach:

```text
Parameterized Queries
```

Testing should begin with understanding parameter behavior rather than immediately attempting destructive database operations.

---

# Command Injection Reference

Conceptually:

```text
User Input
     ↓
Shell Command Construction
     ↓
Operating-System Shell
```

If user input is interpreted as command syntax, command injection may occur.

Defensive approach:

```text
Avoid shell invocation
Use structured APIs
Strict validation
Least privilege
```

Authorized testing should use non-destructive proof techniques.

---

# Path Traversal Reference

Conceptually:

```text
User-Controlled File Path
        ↓
Insufficient Path Validation
        ↓
Access Outside Intended Directory
```

Security boundary:

```text
Allowed Directory
      vs
Outside Filesystem
```

Testing should avoid retrieving sensitive data beyond what is necessary to demonstrate the issue.

---

# SSRF Reference

**SSRF = Server-Side Request Forgery**

Conceptually:

```text
Attacker-Controlled URL
       ↓
Server
       ↓
Server Makes Request
       ↓
Unexpected Destination
```

Potential targets can include:

```text
Internal services
Cloud metadata
Local services
Other backend systems
```

SSRF testing can cross infrastructure boundaries, so scope control is especially important.

---

# SSTI Reference

**SSTI = Server-Side Template Injection**

Conceptually:

```text
User Input
     ↓
Template Engine
     ↓
Input Interpreted as Template Syntax
```

Impact depends on:

```text
Template engine
Sandboxing
Available functions
Application permissions
```

---

# XXE Reference

**XXE = XML External Entity**

Relevant when an application parses XML with unsafe external-entity behavior.

Conceptually:

```text
XML Input
    ↓
XML Parser
    ↓
External Entity Processing
    ↓
Unexpected Resource Access
```

Modern secure XML parsers/configurations should disable unnecessary external entity resolution.

---

# File Upload Testing

Upload security should evaluate:

```text
File type validation
Filename handling
Storage location
Authorization
Content processing
Size limits
Execution permissions
```

The objective is to determine whether uploaded content can violate an intended security boundary.

Use harmless test files.

---

# Open Redirect

Conceptually:

```text
Trusted Application
       ↓
User-Controlled Destination
       ↓
Redirect
       ↓
External Site
```

Potential impact can include:

```text
Phishing
Trust abuse
OAuth flow issues
```

depending on context.

---

# JWT

**JWT = JSON Web Token**

Common security-review areas include:

```text
Signature verification
Algorithm configuration
Expiration
Issuer
Audience
Key management
Authorization claims
```

Do not assume:

```text
JWT decoded
=
JWT compromised
```

JWT payloads are commonly encoded, not encrypted.

---

# GraphQL

GraphQL security review can include:

```text
Authorization
Object access
Query complexity
Introspection exposure
Information disclosure
Batching
Rate limiting
```

The existence of introspection alone is not automatically a vulnerability.

---

# NoSQL Injection

Conceptually:

```text
User Input
     ↓
Unsafe Query Construction
     ↓
NoSQL Database
     ↓
Unexpected Query Behavior
```

The exact testing method depends heavily on the database and application parser.

---

# LDAP Injection

Conceptually:

```text
User Input
     ↓
LDAP Filter Construction
     ↓
Directory Query
```

Defenses include:

```text
Correct escaping
Structured APIs
Input validation
Least privilege
```

---

# Encoding

Payload references often include encoding variants:

```text
URL Encoding
HTML Encoding
Unicode
Base64
Double Encoding
```

Important:

```text
Encoding != Encryption
```

Encoding differences matter because applications may decode data at different processing layers.

---

# Bypass Techniques

Repositories may document bypass concepts for:

```text
Filters
WAFs
Parsers
Blacklists
Validation logic
```

Do not treat bypass lists as:

```text
Random strings to spam
```

Instead understand:

```text
Which parser?
Which normalization step?
Which security control?
Why should this variation matter?
```

---

# WAF

**WAF = Web Application Firewall**

A WAF can detect/block certain requests.

But:

```text
WAF
!=
Replacement for secure code
```

Security must ultimately be enforced by the application and its supporting components.

---

# Automation

Payload collections can be integrated into tools such as:

```text
Burp Intruder
FFUF
Custom scripts
Fuzzers
```

However, automated payload testing can create significant traffic and unexpected application behavior.

Use:

```text
Relevant subset
+
Rate control
+
Explicit authorization
```

---

# Avoid Blind Payload Spraying

Bad methodology:

```text
Find Parameter
     ↓
Send Thousands of Payloads
     ↓
Look for Weird Response
```

Better:

```text
Understand Parameter
      ↓
Determine Context
      ↓
Form Hypothesis
      ↓
Choose Relevant Tests
      ↓
Validate
```

This reduces:

```text
Noise
False positives
Traffic
Application risk
```

---

# Payload Result != Vulnerability

Suppose a test produces:

```text
500 Internal Server Error
```

This means:

```text
Application encountered an error
```

It does not automatically prove:

```text
SQL Injection
Command Injection
RCE
```

You must identify the root cause.

---

# False Positives

Possible causes:

```text
Generic server errors
Input validation
WAF behavior
Application bugs unrelated to vulnerability
Unexpected parser errors
Rate limiting
```

Manual validation is essential.

---

# Defensive Value

PayloadsAllTheThings is also useful for defenders.

AppSec teams can use it to understand:

```text
How vulnerabilities are tested
Common input patterns
Parser edge cases
Security-control assumptions
```

This helps improve:

```text
Secure coding
Test cases
WAF tuning
Detection engineering
```

---

# Reference Workflow

For a suspected vulnerability:

```text
Identify Vulnerability Class
        ↓
Read Relevant Reference
        ↓
Understand Root Cause
        ↓
Choose Minimal Safe Test
        ↓
Use Burp / Caido
        ↓
Observe Behavior
        ↓
Confirm or Reject Hypothesis
```

---

# PayloadsAllTheThings vs SecLists

```text
PayloadsAllTheThings
       |
Vulnerability-specific
payloads + methodology
```

```text
SecLists
       |
General-purpose security
wordlists and test data
```

They overlap in some areas but have different primary purposes.

---

# PayloadsAllTheThings vs OneListForAll

```text
OneListForAll
      |
Web path discovery
```

versus:

```text
PayloadsAllTheThings
      |
Vulnerability testing references
```

One asks:

```text
"What path might exist?"
```

The other helps answer:

```text
"How can this suspected
vulnerability class be tested?"
```

---

# Security Testing Philosophy

The correct process is:

```text
INPUT
  ↓
CONTEXT
  ↓
HYPOTHESIS
  ↓
RELEVANT PAYLOAD
  ↓
OBSERVATION
  ↓
VALIDATION
  ↓
IMPACT
```

Not:

```text
Payload List
    ↓
Spam Everything
```

---

# Key Takeaway

PayloadsAllTheThings should be treated as:

```text
REFERENCE LIBRARY
       +
TESTING KNOWLEDGE
       +
PAYLOAD COLLECTION
```

Its greatest value is not memorizing payload strings.

Learn:

```text
WHY the payload works
       ↓
WHICH parser interprets it
       ↓
WHAT security boundary it tests
       ↓
HOW to confirm the result safely
```

The goal of application security testing is not:

```text
"Make a payload execute."
```

It is:

```text
Understand the root cause,
prove the security impact,
and identify how the application
should prevent it.
```
