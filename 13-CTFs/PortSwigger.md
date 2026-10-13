
# PortSwigger Web Security Academy

> Practical web-application security training covering HTTP, authentication, authorization, injection vulnerabilities, and advanced web exploitation concepts.

## 1. What is PortSwigger Web Security Academy?

**PortSwigger Web Security Academy** is a free web-security training platform developed by the creators of Burp Suite.

Website: https://portswigger.net/web-security

It provides structured learning material and interactive vulnerable labs.

Major areas:

```text
SQL Injection
Cross-Site Scripting
Authentication
Access Control
CSRF
SSRF
XXE
File Upload
Path Traversal
Business Logic
API Testing
```

---

## 2. Recommended Tools

Primary:

```text
Burp Suite
```

Other useful tools:

```text
Browser Developer Tools
Caido
curl
Python
```

Burp Repeater is particularly valuable for manual request testing.

---

## 3. Standard Lab Workflow

```text
Read Lab Objective
      ↓
Understand Vulnerability Class
      ↓
Browse Application
      ↓
Capture HTTP Traffic
      ↓
Identify Relevant Input
      ↓
Test Hypothesis
      ↓
Validate Behavior
      ↓
Complete Lab
```

---

## 4. HTTP Fundamentals

Request example:

```http
GET /products?id=100 HTTP/1.1
Host: lab.example.test
Cookie: session=example
```

Important components:

```text
Method
Path
Parameters
Headers
Cookies
Body
```

Response:

```http
HTTP/1.1 200 OK
Content-Type: text/html
```

---

## 5. Burp Proxy

Conceptually:

```text
Browser
   ↓
Burp Proxy
   ↓
Lab Application
```

Use Proxy to inspect traffic and identify relevant requests.

---

## 6. Burp Repeater

Core workflow:

```text
Captured Request
       ↓
Send to Repeater
       ↓
Modify Input
       ↓
Send
       ↓
Compare Response
```

This supports controlled manual security testing.

---

## 7. SQL Injection

SQLi occurs when untrusted input affects SQL query structure.

Conceptually:

```text
Input
  ↓
Unsafe SQL Construction
  ↓
Database
```

Lab learning objectives:

```text
Understand query context
Identify unsafe input handling
Observe database behavior
Understand parameterized queries
```

Defenses:

```text
Prepared statements
Bound parameters
Least-privileged database accounts
```

---

## 8. Cross-Site Scripting

XSS occurs when untrusted input is interpreted as executable browser content.

Types:

```text
Reflected
Stored
DOM-Based
```

Conceptually:

```text
Untrusted Input
      ↓
Unsafe Rendering
      ↓
Browser Execution
```

Harmless lab proof:

```html
<script>alert('XSS')</script>
```

Use only within the provided training lab.

---

## 9. Access Control

Access-control vulnerabilities occur when applications fail to enforce permissions correctly.

Examples:

```text
IDOR / BOLA
Vertical privilege escalation
Horizontal privilege escalation
Function-level authorization failure
```

Important question:

```text
Does the server verify that
the current user may access
this specific resource?
```

---

## 10. IDOR / BOLA

Example:

```http
GET /api/orders/100 HTTP/1.1
Host: lab.example.test
```

In a controlled lab with two test accounts, investigate whether object access is restricted to the correct account.

The vulnerability exists when server-side authorization is missing or insufficient.

---

## 11. Authentication

Important topics:

```text
Login
Logout
Password reset
MFA
Session management
Account recovery
```

Review:

```text
Rate limiting
Session rotation
Token expiration
Credential validation
```

---

## 12. CSRF

**CSRF = Cross-Site Request Forgery**

Conceptually:

```text
Victim Browser
      ↓
Unwanted Authenticated Request
      ↓
Application
```

Defenses:

```text
CSRF tokens
SameSite cookies
Origin validation
Safe HTTP method semantics
```

---

## 13. SSRF

**SSRF = Server-Side Request Forgery**

Conceptually:

```text
User-Controlled URL
       ↓
Application Server
       ↓
Server Makes Request
```

Potential security issue:

```text
Server accesses resources
outside the intended trust boundary
```

Testing must remain within the lab's authorized infrastructure.

---

## 14. Path Traversal

Occurs when unsafe file-path handling permits access outside the intended directory.

Conceptually:

```text
User Input
    ↓
File Path Construction
    ↓
Unexpected Filesystem Location
```

Defenses:

```text
Canonical path validation
Restricted directories
Allowlisted identifiers
Least privilege
```

---

## 15. File Upload Vulnerabilities

Review:

```text
File type validation
Content validation
Storage permissions
Filename handling
Authorization
Execution restrictions
```

Use harmless lab files for validation.

---

## 16. XXE

**XXE = XML External Entity**

Occurs when unsafe XML parsing processes external entities.

Conceptually:

```text
XML Input
   ↓
XML Parser
   ↓
External Resource Resolution
```

Defenses include disabling unnecessary external entity resolution.

---

## 17. SSTI

**SSTI = Server-Side Template Injection**

Occurs when untrusted input is interpreted as template syntax.

Conceptually:

```text
Input
  ↓
Template Engine
  ↓
Unexpected Evaluation
```

Impact depends on the template engine and sandbox configuration.

---

## 18. Business Logic Vulnerabilities

Business logic flaws arise from incorrect application rules.

Examples:

```text
Invalid workflow transitions
Incorrect price calculations
Missing authorization checks
Broken transaction limits
```

These often require manual reasoning rather than automated scanners.

---

## 19. API Security

Important areas:

```text
BOLA
Broken authentication
Mass assignment
Excessive data exposure
Rate limiting
Function-level authorization
```

Understand JSON requests and API object relationships.

---

## 20. CORS

**CORS = Cross-Origin Resource Sharing**

Important headers:

```http
Access-Control-Allow-Origin:
Access-Control-Allow-Credentials:
```

Review how origins are validated and whether credentials can be sent cross-origin.

---

## 21. JWT

JWT structure:

```text
header.payload.signature
```

Review:

```text
Signature verification
Algorithm restrictions
Expiration
Issuer
Audience
Key management
Authorization claims
```

Decoding a JWT is not the same as compromising it.

---

## 22. WebSockets

WebSockets enable persistent bidirectional communication.

Conceptually:

```text
Client
   ↔
WebSocket Connection
   ↔
Server
```

Security review:

```text
Authentication
Authorization
Message validation
Origin restrictions
Session expiration
```

---

## 23. HTTP Request Smuggling

Request smuggling can arise when HTTP components disagree about message boundaries.

Conceptually:

```text
Frontend Proxy
      ↓
Different Parsing Interpretation
      ↓
Backend Server
```

This is an advanced topic requiring careful isolated testing.

---

## 24. Web Cache Vulnerabilities

Important concepts:

```text
Cache poisoning
Cache deception
Cache key construction
Unkeyed inputs
Response caching
```

Security depends on consistent interpretation of requests and cache policy.

---

## 25. Lab Difficulty Progression

Recommended learning sequence:

```text
Apprentice
    ↓
Practitioner
    ↓
Expert
```

Start with fundamentals before advanced parser and protocol vulnerabilities.

---

## 26. Recommended Learning Order

```text
HTTP Basics
    ↓
SQL Injection
    ↓
XSS
    ↓
Authentication
    ↓
Access Control
    ↓
CSRF
    ↓
Path Traversal
    ↓
File Upload
    ↓
SSRF / XXE / SSTI
    ↓
Advanced Web Vulnerabilities
```

---

## 27. Documentation Template

```text
Lab Name:
__________

Topic:
__________

Difficulty:
__________

Vulnerable Endpoint:
__________

Input:
__________

Root Cause:
__________

Testing Method:
__________

Observed Behavior:
__________

Security Impact:
__________

Mitigation:
__________
```

---

## 28. Common Mistakes

Avoid:

```text
Copying payloads without context
Ignoring HTTP response differences
Assuming 500 errors prove exploitation
Skipping authorization analysis
Using automation without understanding
Ignoring root cause
```

---

## Key Takeaway

PortSwigger Web Security Academy develops practical application-security reasoning.

```text
HTTP UNDERSTANDING
       +
INPUT ANALYSIS
       +
MANUAL TESTING
       +
ROOT CAUSE
       =
WEB SECURITY SKILL
```

The most important tool is not a payload list—it is understanding how application trust boundaries work.
