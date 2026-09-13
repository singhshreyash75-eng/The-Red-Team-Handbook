# Scripting

> Scripting and programming fundamentals for cybersecurity automation using Bash, Go, PowerShell, and Python.

## Module Overview

Security work involves repetitive tasks such as:

```text
Reconnaissance
Enumeration
HTTP requests
File processing
Log analysis
API interaction
Output normalization
Network testing
Reporting
```

Scripting converts these repetitive tasks into:

```text
Repeatable
Reliable
Reusable
Automated
```

workflows.

---

# Files

```text
Bash.md
Go.md
PowerShell.md
Python.md
README.md
```

---

# Languages Covered

```text
Bash
PowerShell
Python
Go
```

Each language has a different strength.

---

# Bash

Best suited for:

```text
Linux automation
Command chaining
Tool orchestration
File processing
Recon pipelines
Quick scripts
```

Conceptually:

```text
Command A
    |
    v
Command B
    |
    v
grep / sort / awk
    |
    v
Output
```

Bash excels when the problem is:

```text
"Run these commands
and connect their outputs."
```

---

# PowerShell

Best suited for:

```text
Windows administration
Active Directory
Microsoft environments
.NET automation
Structured system data
Cloud administration
```

Its major advantage is:

```text
Object-Based Pipeline
```

Conceptually:

```text
Cmdlet
  ↓
Objects
  ↓
Filter Properties
  ↓
Select Properties
```

---

# Python

Best suited for:

```text
General automation
Networking
HTTP / APIs
Data processing
Security tools
Complex scripting
Parsing
Rapid development
```

Python sits between simple shell scripting and larger software development.

---

# Go

Best suited for:

```text
High-performance tooling
Networking
Concurrency
Portable CLI tools
Large scanners
Cloud tooling
```

Go is especially common in modern reconnaissance tooling.

---

# Language Comparison

```text
Bash
 |
Linux + CLI automation
 |
Fastest for shell pipelines


PowerShell
 |
Windows + AD automation
 |
Strong Microsoft integration


Python
 |
General-purpose automation
 |
Fast development + huge ecosystem


Go
 |
Compiled tooling
 |
Performance + concurrency + portability
```

---

# Choosing a Language

Ask:

```text
What am I trying to automate?
```

### Shell Commands?

Use:

```text
Bash
```

### Windows / Active Directory?

Use:

```text
PowerShell
```

### APIs / Parsing / Complex Logic?

Use:

```text
Python
```

### Fast Concurrent Standalone Tool?

Consider:

```text
Go
```

---

# Core Programming Concepts

Regardless of language, understand:

```text
Variables
Data Types
Conditions
Loops
Functions
Input / Output
Files
Errors
Arguments
Data Structures
Networking
```

Syntax changes.

The concepts remain largely transferable.

---

# Variables

Concept:

```text
Name
  ↓
Value
```

Examples:

Bash:

```bash
domain="example.com"
```

PowerShell:

```powershell
$domain = "example.com"
```

Python:

```python
domain = "example.com"
```

Go:

```go
domain := "example.com"
```

---

# Conditions

Generic logic:

```text
IF condition
    do something
ELSE
    do something else
```

Used heavily in:

```text
Error handling
Filtering
Decision making
Validation
```

---

# Loops

Generic concept:

```text
For every target
      ↓
Perform action
```

Example:

```text
targets.txt
     |
     +--> Target 1
     +--> Target 2
     +--> Target 3
```

Loops eliminate repetitive manual execution.

---

# Functions

Functions provide:

```text
Reusable Logic
```

Instead of:

```text
Copy same code repeatedly
```

use:

```text
Function
   ↓
Call whenever required
```

This improves maintainability.

---

# Input

Scripts may receive input through:

```text
Command-line arguments
Files
stdin
Environment variables
APIs
Configuration files
```

Never assume input is trustworthy.

---

# Output

Useful output formats include:

```text
Plain text
JSON
CSV
Logs
```

For automation pipelines, structured output such as:

```text
JSON
```

is often easier to process reliably than human-formatted text.

---

# Exit Codes

CLI tools should communicate success/failure.

Conceptually:

```text
0
=
Success
```

and:

```text
Non-zero
=
Failure / special condition
```

This allows scripts to chain tools reliably.

---

# Error Handling

Bad automation:

```text
Error occurs
     ↓
Ignored
     ↓
Incorrect output
```

Better:

```text
Error occurs
     ↓
Detect
     ↓
Handle / Log
     ↓
Continue safely or stop
```

Error handling is a core security-tooling skill.

---

# File Processing

Security workflows frequently involve:

```text
domains.txt
subdomains.txt
urls.txt
live.txt
findings.json
logs.txt
```

Typical pipeline:

```text
Raw Data
   ↓
Parse
   ↓
Normalize
   ↓
Deduplicate
   ↓
Filter
   ↓
Output
```

---

# Structured Data

Modern tools increasingly use:

```text
JSON
CSV
YAML
```

instead of only plain text.

Example:

```text
Scanner
   ↓
JSON
   ↓
Python / PowerShell
   ↓
Filtered Results
```

Understanding structured data dramatically improves automation capability.

---

# APIs

Cloud platforms, security tools, and SaaS products expose APIs.

Conceptually:

```text
Script
   ↓
HTTP Request
   ↓
API
   ↓
JSON Response
   ↓
Script
```

Python and PowerShell are particularly convenient for API automation.

---

# HTTP Fundamentals

Automation often interacts with:

```text
GET
POST
PUT
PATCH
DELETE
```

and:

```text
Headers
Cookies
Tokens
JSON bodies
Status codes
```

Understanding HTTP is therefore important even when scripting rather than manually testing applications.

---

# Networking

Security scripts may work with:

```text
IP addresses
Ports
DNS
TCP
UDP
HTTP
TLS
```

Always implement:

```text
Timeouts
Rate limits
Error handling
```

for network operations.

---

# Concurrency

Large security workflows may process many targets.

Sequential:

```text
Target 1
   ↓
Target 2
   ↓
Target 3
```

Concurrent:

```text
      +--> Target 1
      |
Tool -+--> Target 2
      |
      +--> Target 3
```

Useful technologies include:

```text
Go goroutines
Python asyncio / threads
PowerShell parallel capabilities
Shell background jobs
```

---

# Concurrency != Unlimited Speed

Increasing concurrency also increases:

```text
Network traffic
Target load
CPU usage
Memory usage
Rate-limit pressure
Detection
```

Therefore:

```text
Controlled Concurrency
```

is more important than maximum concurrency.

---

# Security Automation Pipeline

Example:

```text
Root Domain
    ↓
Subdomain Discovery
    ↓
Deduplicate
    ↓
HTTP Validation
    ↓
URL Collection
    ↓
Security Checks
    ↓
Normalize Results
    ↓
Manual Validation
```

Scripting allows this workflow to become repeatable.

---

# Bash Pipeline Example

Conceptually:

```bash
subfinder -d example.com -silent |
sort -u |
httpx
```

This demonstrates the Unix philosophy:

```text
Small Tools
    +
Pipes
    =
Larger Workflow
```

---

# Python Automation Example

When the workflow needs:

```text
APIs
Conditional logic
JSON
Error handling
Concurrency
State
```

Python becomes more appropriate.

Conceptually:

```text
Input
 ↓
Python
 |
 +-- API Request
 +-- Parse JSON
 +-- Validate
 +-- Deduplicate
 +-- Store Result
```

---

# PowerShell Automation Example

For Windows environments:

```text
Domain
 ↓
PowerShell
 |
 +-- Users
 +-- Groups
 +-- Computers
 +-- Services
 +-- Events
```

PowerShell's object model reduces fragile text parsing.

---

# Go Tool Architecture

Many high-performance recon tools follow:

```text
Input
 ↓
Worker Pool / Goroutines
 ↓
Network Requests
 ↓
Response Parsing
 ↓
Structured Output
```

This is a natural fit for Go.

---

# Input Validation

Never blindly trust:

```text
User input
File input
API responses
Command-line arguments
Environment variables
```

Validate:

```text
Expected format
Length
Allowed values
Path
URL
IP address
```

where appropriate.

---

# Command Injection

A major scripting risk:

```text
Untrusted Input
      ↓
Shell Command String
      ↓
Shell
      ↓
Unexpected Execution
```

Avoid unnecessary:

```text
eval
shell=True
Invoke-Expression
```

with untrusted data.

Prefer structured APIs and argument arrays.

---

# Secrets

Scripts frequently need:

```text
API keys
Tokens
Passwords
Certificates
Cloud credentials
```

Never hardcode secrets into committed source code.

Prefer:

```text
Environment variables
Secret managers
Managed identities
Credential stores
```

---

# Logging

Good scripts record:

```text
Timestamp
Operation
Target
Result
Error
```

but should avoid logging:

```text
Passwords
Session tokens
Private keys
Sensitive personal data
```

unless explicitly required and protected.

---

# Dependencies

Third-party libraries introduce:

```text
Supply-chain risk
Vulnerabilities
Version conflicts
Malicious packages
```

Use:

```text
Trusted repositories
Pinned versions
Dependency review
Virtual environments
Go modules
```

where appropriate.

---

# Reproducibility

A good security script should behave consistently.

Document:

```text
Language version
Dependencies
Arguments
Input format
Output format
Required permissions
```

This turns:

```text
Personal Script
```

into:

```text
Reusable Tool
```

---

# Tool Development Workflow

A practical progression:

```text
Manual Task
     ↓
Shell One-Liner
     ↓
Bash Script
     ↓
Python / PowerShell Script
     ↓
Structured Tool
     ↓
Go / Larger Implementation
```

Not every script needs to become a large program.

---

# Automation vs Understanding

Important:

```text
Automation
!=
Understanding
```

If a script runs:

```text
Subfinder
httpx
Nuclei
```

you should still understand:

```text
What each tool sends
What each result means
What can cause false positives
What the security impact is
```

---

# Security Tool Philosophy

A good security tool should ideally provide:

```text
Clear input
Clear output
Timeouts
Rate control
Error handling
Logging
Scope control
Reproducibility
```

---

# Red-Team Use

Scripting can automate authorized workflows such as:

```text
Recon data processing
Asset normalization
HTTP validation
Evidence collection
Report preparation
Lab automation
Tool orchestration
```

The objective is:

```text
Reduce repetitive work
```

while keeping human analysis for decisions requiring context.

---

# Blue-Team Use

The same languages are useful for:

```text
Log parsing
Threat hunting
IOC processing
Incident response
Asset inventory
API automation
Security monitoring
Configuration auditing
```

Scripting is therefore a fundamental cybersecurity skill regardless of specialization.

---

# Recommended Learning Priority

For security work:

```text
1. Bash
      ↓
Linux workflow automation

2. Python
      ↓
General security automation

3. PowerShell
      ↓
Windows / AD automation

4. Go
      ↓
High-performance tool development
```

The exact order can change depending on specialization.

---

# Mental Model

```text
                  SCRIPTING
                      |
        +-------------+-------------+
        |             |             |
      Bash         Python      PowerShell
        |             |             |
 Linux CLI      General Logic    Windows/AD
        |             |             |
        +-------------+-------------+
                      |
                     Go
                      |
           High-Performance Tools
```

---

# Key Takeaway

Do not learn scripting by memorizing syntax alone.

Learn how to transform:

```text
MANUAL TASK
     ↓
ALGORITHM
     ↓
SCRIPT
     ↓
REPEATABLE WORKFLOW
```

The four languages in this module serve different roles:

```text
Bash
=
Glue commands together

Python
=
Automate complex logic

PowerShell
=
Control Windows/Microsoft environments

Go
=
Build fast, concurrent, portable tools
```

For cybersecurity, the most valuable scripting ability is:

```text
Take repetitive technical work
        ↓
Turn it into reliable automation
        ↓
Keep human analysis for decisions
that require security context.
```
