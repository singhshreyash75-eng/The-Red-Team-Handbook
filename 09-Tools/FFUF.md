# FFUF

> FFUF fundamentals for authorized web-content discovery, virtual-host discovery, and HTTP fuzzing.

## What is FFUF?

**FFUF = Fuzz Faster U Fool**

FFUF is a fast HTTP fuzzing tool.

It sends requests while replacing a special keyword:

```text
FUZZ
```

with values from a wordlist.

Conceptually:

```text
Wordlist
   |
   v
  FUZZ
   |
   v
HTTP Request
   |
   v
Responses
```

---

# Basic Idea

Suppose:

```text
https://example.test/FUZZ
```

and wordlist contains:

```text
admin
login
api
backup
```

FFUF tests:

```text
https://example.test/admin
https://example.test/login
https://example.test/api
https://example.test/backup
```

Use this only against authorized targets.

---

# Basic Directory Discovery

Typical syntax:

```bash
ffuf -w wordlist.txt -u https://example.test/FUZZ
```

Important options:

```text
-w    wordlist
-u    URL
```

---

# FUZZ Keyword

`FUZZ` identifies where wordlist values should be inserted.

Example:

```bash
ffuf -w words.txt -u https://example.test/FUZZ
```

But `FUZZ` can appear elsewhere too.

Conceptually:

```text
Path
Query parameter
Header
Host
Request body
```

---

# Wordlists

Wordlist quality matters.

Common wordlist categories:

```text
Directories
Files
API endpoints
Subdomains
Parameters
Technology-specific paths
```

On security-testing distributions you may encounter collections such as:

```text
SecLists
```

Always select a list appropriate to the application.

---

# Directory Discovery

Example:

```bash
ffuf -w directories.txt -u https://example.test/FUZZ
```

Potential results:

```text
admin
api
uploads
assets
backup
```

These are discovered resources, not vulnerabilities.

---

# File Discovery

A wordlist can include filenames:

```text
index.php
config.php
robots.txt
backup.zip
```

or extensions can be tested where appropriate.

The objective is to identify accessible application content.

---

# Extensions

Applications may use technologies such as:

```text
.php
.aspx
.jsp
.html
.txt
.json
```

FFUF supports workflows that test likely extensions.

Avoid blindly testing huge extension lists; first fingerprint the application.

---

# Status Codes

FFUF displays HTTP response codes.

Common examples:

```text
200 OK
204 No Content
301 Redirect
302 Redirect
401 Unauthorized
403 Forbidden
404 Not Found
500 Server Error
```

Do not automatically discard:

```text
401
403
```

because they can reveal valid protected resources.

---

# Filtering

One of FFUF's most important features is filtering noisy responses.

You can filter based on properties such as:

```text
Status
Size
Words
Lines
```

The exact option syntax should be checked with:

```bash
ffuf -h
```

for the installed version.

---

# Response Size

Suppose every nonexistent path returns:

```text
200 OK
Size: 4242
```

This application uses a custom error page.

If you search only for status `200`, nearly everything looks valid.

Instead:

```text
Identify baseline error response
       ↓
Filter its size/content
       ↓
Interesting responses remain
```

---

# Baseline First

Before fuzzing, understand:

```text
What does a nonexistent page look like?
```

Example:

```text
/random-definitely-not-real
```

Observe:

```text
Status
Length
Words
Redirect behavior
```

This helps identify false positives.

---

# Auto Calibration

FFUF versions may support automatic calibration behavior that helps identify common baseline responses.

Check:

```bash
ffuf -h
```

for available calibration options.

Still manually understand the application's response behavior.

---

# Query Parameter Fuzzing

`FUZZ` can appear in a query value:

```text
https://example.test/search?q=FUZZ
```

Conceptually:

```text
Wordlist
   ↓
Parameter Values
   ↓
Server Responses
```

This is useful for controlled input-behavior testing.

---

# Parameter Name Discovery

Conceptually, you may test candidate parameter names:

```text
/api?FUZZ=test
```

with a wordlist containing:

```text
id
user
page
debug
lang
```

The goal is to determine whether undocumented parameters influence application behavior.

Use conservative rates.

---

# POST Requests

FFUF can also fuzz request bodies in authorized applications.

Conceptually:

```http
POST /api/search
Content-Type: application/x-www-form-urlencoded

query=FUZZ
```

The exact command depends on the request format and installed FFUF version.

---

# Headers

The fuzz marker can also conceptually be placed in HTTP headers.

Example use cases include testing:

```text
Virtual hosts
Application-specific headers
Routing behavior
```

Do not blindly fuzz security-sensitive headers without understanding the application.

---

# Virtual Host Discovery

Multiple websites may share one IP.

Conceptually:

```text
IP Address
    |
    +-- www.example.test
    +-- admin.example.test
    +-- dev.example.test
```

HTTP routing can depend on:

```http
Host: ...
```

Virtual-host discovery tests candidate hostnames against an authorized server.

---

# VHost Concept

Request:

```http
GET / HTTP/1.1
Host: app.example.test
```

The same IP may respond differently to:

```http
Host: admin.example.test
```

This can reveal additional applications.

---

# Recursion

Directory discovery may reveal:

```text
/api
```

which itself contains:

```text
/api/v1
/api/admin
/api/docs
```

Recursive discovery can explore deeper paths.

But recursion increases:

```text
Traffic
Requests
Runtime
```

Use it deliberately.

---

# Rate Control

Fuzzing can generate large request volumes.

This can affect:

```text
Application performance
Logs
WAF
Rate limits
```

Therefore:

```text
Fastest possible
```

is not always:

```text
Best
```

Respect the engagement's rules and application capacity.

---

# Threads / Concurrency

Higher concurrency:

```text
More Requests
     ↓
Faster Enumeration
```

but also:

```text
Higher Server Load
Higher Detection
Possible Rate Limiting
```

Use reasonable values.

---

# Authentication

Some content exists only after authentication.

FFUF can send authorized session material such as cookies or headers where the engagement permits it.

Conceptually:

```text
Authenticated Session
       ↓
FFUF
       ↓
Authenticated Content Discovery
```

Protect session tokens in shell history and logs.

---

# Proxying Through Burp/Caido

For debugging, HTTP requests can be routed through an intercepting proxy where supported.

Conceptually:

```text
FFUF
 ↓
Burp / Caido
 ↓
Target
```

This helps inspect generated requests.

Avoid proxying massive fuzzing runs through a GUI unless necessary.

---

# FFUF + Burp

Recommended workflow:

```text
FFUF
 ↓
Interesting Path
 ↓
Burp Repeater
 ↓
Manual Validation
```

Example:

```text
/admin
/api/debug
/internal
```

Discovery itself is not a vulnerability.

---

# FFUF + Caido

Same concept:

```text
FFUF
 ↓
Endpoint Discovery
 ↓
Caido
 ↓
Inspect + Replay
```

---

# FFUF + httpx

Recon pipeline:

```text
Subdomains
   ↓
httpx
   ↓
Live Web Apps
   ↓
FFUF
   ↓
Content Discovery
```

---

# FFUF vs Gobuster

Both can perform content discovery.

```text
Gobuster
   |
Directory / DNS / VHost enumeration
```

```text
FFUF
   |
Flexible HTTP fuzzing using FUZZ positions
```

FFUF's flexible insertion point makes it useful beyond directory enumeration.

---

# FFUF vs Feroxbuster

```text
FFUF
 |
General-purpose HTTP fuzzing
```

```text
Feroxbuster
 |
Recursive content discovery
```

Both are valuable for different workflows.

---

# False Positives

Common causes:

```text
Custom 404 pages
Wildcard routing
Redirects
Authentication pages
WAF responses
Same response for every path
```

Always compare:

```text
Status
Size
Words
Lines
Content
```

---

# Enumeration Workflow

```text
1. Identify target technology

2. Establish baseline response

3. Choose appropriate wordlist

4. Start low-noise discovery

5. Filter baseline responses

6. Review 200/3xx/401/403 results

7. Investigate interesting paths

8. Recurse only where useful

9. Validate manually
```

---

# Useful Mental Model

```text
WORDLIST
    ↓
   FUZZ
    ↓
REQUEST
    ↓
SERVER
    ↓
RESPONSE
    ↓
FILTER
    ↓
INTERESTING RESULT
```

---

# Key Takeaway

FFUF is not simply:

```text
Directory brute-forcer
```

Its core concept is:

```text
Replace FUZZ
with candidate values
inside HTTP requests
```

This makes it useful for:

```text
Content discovery
VHost discovery
Parameter discovery
Controlled input fuzzing
```

But always:

```text
Automated Discovery
       ↓
Manual Validation
```

A different HTTP response is a lead, not automatically a vulnerability.
