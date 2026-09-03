# Searchsploit

> Command-line Exploit-DB search and vulnerability research notes for authorized labs and assessments.

## What is Searchsploit?

**Searchsploit** is a command-line utility used to search a local copy/index of:

```text
Exploit-DB
```

Instead of manually searching a website:

```text
Terminal
   |
Searchsploit
   |
Local Exploit-DB Data
   |
Matching Exploits / PoCs
```

It is particularly useful during:

```text
CTFs
Pentesting labs
Vulnerability research
Offline exploit research
```

---

# Searchsploit vs Exploit-DB

```text
Exploit-DB
=
Exploit/PoC database
```

```text
Searchsploit
=
CLI used to search that data
```

---

# Basic Search

Syntax:

```bash
searchsploit <search-term>
```

Example:

```bash
searchsploit apache
```

Another:

```bash
searchsploit openssh
```

---

# Product + Version

More useful:

```bash
searchsploit "Apache 2.4"
```

Example:

```bash
searchsploit "OpenSSH 7.2"
```

General methodology:

```text
Product
   +
Version
   ↓
Searchsploit
```

---

# Search by CVE

```bash
searchsploit CVE-XXXX-YYYY
```

This can locate Exploit-DB entries associated with a known CVE.

Remember:

```text
No Searchsploit Result
        !=
No vulnerability exists
```

It only means the local Exploit-DB data did not return a matching entry.

---

# Search Results

Typical output conceptually contains:

```text
Exploit Title
Path
```

Example structure:

```text
Product X - Vulnerability
--------------------------------
path/to/exploit.py
```

The path identifies the local exploit file.

---

# Exact Search

Searchsploit supports options to refine searches.

Use help:

```bash
searchsploit -h
```

This is preferable to guessing options when working across versions.

---

# Copy Exploit Locally

A useful workflow is to copy the selected exploit into the current working directory before modifying it.

Conceptually:

```text
Exploit-DB Copy
      ↓
Working Copy
      ↓
Review / Modify
```

This keeps the original database copy unchanged.

---

# Inspect Before Running

Always:

```bash
less exploit.py
```

or:

```bash
cat exploit.py
```

For C:

```bash
less exploit.c
```

Look for:

```text
Target addresses
Hardcoded ports
Shell commands
Network callbacks
Dependencies
File modifications
Destructive actions
```

---

# Public Exploits Are Untrusted

Critical rule:

```text
NEVER blindly execute
public exploit code.
```

An exploit file can contain:

```text
Malicious commands
Backdoors
Credential theft
Destructive behavior
Unexpected callbacks
```

Treat downloaded/public code as untrusted.

---

# Product Matching

Suppose enumeration finds:

```text
OpenSSH 7.2p2
```

Search:

```bash
searchsploit "OpenSSH 7.2"
```

But a search result does not prove applicability.

Verify:

```text
Operating system
Exact version
Patch status
Architecture
Configuration
Authentication requirements
```

---

# Broad Search Problem

Searching:

```bash
searchsploit apache
```

may return many irrelevant results.

Improve searches using:

```text
Product
Version
Component
Vulnerability type
```

Example:

```bash
searchsploit "Apache 2.4"
```

---

# Version Is Not Enough

Especially on Linux:

```text
Old upstream version
      ↓
Distribution backported patch
      ↓
Not necessarily vulnerable
```

Check:

```text
Vendor advisory
Distribution security tracker
Package changelog
```

---

# Searchsploit Workflow

Recommended:

```text
Nmap / Enumeration
       ↓
Product
       ↓
Version
       ↓
CVE Research
       ↓
Searchsploit
       ↓
Potential PoC
       ↓
Read Source
       ↓
Check Applicability
       ↓
Controlled Validation
```

---

# Nmap Integration Concept

Network enumeration may identify:

```text
Port
Service
Version
```

Example:

```text
80/tcp
HTTP
Product X
Version Y
```

Then:

```text
Product X Version Y
       ↓
CVE / Vendor Research
       ↓
Searchsploit
```

Do not jump directly from an open port to exploitation.

---

# Exploit Path

Searchsploit results point to local exploit files.

The file may be:

```text
.py
.c
.rb
.pl
.sh
.txt
```

Different extensions indicate different execution/research requirements.

---

# Python Exploit

Before running:

```bash
python3 exploit.py
```

first inspect:

```bash
head -n 50 exploit.py
less exploit.py
```

Check whether it expects:

```text
Python 2
or
Python 3
```

Older Exploit-DB material may require older environments.

---

# C Exploit

For source such as:

```text
exploit.c
```

review first.

Then, in an isolated authorized lab, compilation may typically use:

```bash
gcc exploit.c -o exploit
```

Check:

```text
Architecture
Compiler warnings
Dependencies
Target assumptions
```

---

# Local vs Remote Exploit

Search results may represent different attack types.

## Remote

```text
Attacker
   |
Network
   |
Target
```

## Local

```text
Existing local access
       |
Exploit
       |
Privilege escalation
```

Do not attempt a local exploit against a remote service simply because product names appear similar.

---

# DoS Exploits

Be particularly careful with:

```text
Denial of Service
```

PoCs.

They may intentionally:

```text
Crash service
Exhaust resources
Reboot target
Corrupt state
```

DoS testing requires explicit authorization and appropriate lab conditions.

---

# Exploit Reliability

Searchsploit does not guarantee:

```text
Exploit works
```

Possible reasons for failure:

```text
Different patch level
Wrong architecture
Wrong configuration
Mitigation enabled
Exploit bug
Different library
Different operating system
```

---

# Failed Exploit

Remember:

```text
Exploit failed
      !=
Target is patched
```

Failure only tells you:

```text
That attempt failed.
```

Investigate the root cause.

---

# Searchsploit vs NVD

```text
NVD
 |
Vulnerability information
```

```text
Searchsploit
 |
Exploit / PoC discovery
```

Use both:

```text
NVD
 ↓
Understand CVE
 ↓
Searchsploit
 ↓
Find public PoC
```

---

# Searchsploit vs Metasploit

```text
Searchsploit
=
Search Exploit-DB
```

```text
Metasploit
=
Exploit-development/testing framework
```

A vulnerability may have:

```text
Exploit-DB PoC
Metasploit module
Both
Neither
```

---

# Useful Research Pattern

```text
searchsploit <product>

searchsploit "<product> <version>"

searchsploit CVE-XXXX-YYYY
```

Then:

```text
Read exploit
      ↓
Research CVE
      ↓
Verify affected versions
      ↓
Check prerequisites
```

---

# Safe Lab Checklist

Before testing:

```text
[ ] Explicit authorization
[ ] Correct target
[ ] Correct product
[ ] Correct version
[ ] Exploit source reviewed
[ ] Dependencies understood
[ ] No unexpected callbacks
[ ] No destructive behavior
[ ] Snapshot available
[ ] Isolated lab
```

---

# Key Takeaway

Searchsploit should fit here:

```text
Enumeration
     ↓
Product + Version
     ↓
CVE Research
     ↓
Searchsploit
     ↓
Exploit Candidate
     ↓
Source Review
     ↓
Applicability Check
     ↓
Controlled Testing
```

Never use:

```text
searchsploit
     ↓
first result
     ↓
run blindly
```

Use it as a **research tool**, not an automatic exploitation engine.
