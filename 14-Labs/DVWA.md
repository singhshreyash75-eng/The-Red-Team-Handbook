
# DVWA — Damn Vulnerable Web Application

> A practical training environment for understanding common web vulnerabilities through controlled, hands-on exercises.

## 1. What is DVWA?

**DVWA (Damn Vulnerable Web Application)** is an intentionally vulnerable PHP/MySQL web application designed for cybersecurity education.

It allows learners to practice identifying and understanding web application vulnerabilities in a controlled environment.

Common topics:

- SQL Injection
- Cross-Site Scripting (XSS)
- Command Injection
- File Inclusion
- File Upload Vulnerabilities
- CSRF
- Authentication Weaknesses
- Brute Force Protection

Official repository: https://github.com/digininja/DVWA

---

## 2. Why Use DVWA?

DVWA provides a structured environment where security controls can be studied at different difficulty levels.

```text
Web Application
      ↓
Vulnerability Category
      ↓
Security Level
      ↓
Testing
      ↓
Understanding Root Cause
```

The goal is to learn why vulnerabilities occur and how secure implementations prevent them.

---

## 3. Security Levels

DVWA commonly provides these security levels:

| Level | Description |
|---|---|
| Low | Minimal security controls |
| Medium | Partial defenses |
| High | Stronger validation |
| Impossible | Reference secure implementation |

Some versions also include JavaScript-related security challenges.

---

## 4. Deployment with Docker

Clone the official repository:

```bash
git clone https://github.com/digininja/DVWA.git
cd DVWA
```

Review the Docker configuration and documentation.

A common deployment command is:

```bash
docker compose up -d
```

Depending on the current Compose configuration, DVWA is commonly accessible through a locally mapped web port.

Check the actual mapping:

```bash
docker compose ps
```

Keep the application bound to loopback or an isolated lab network.

---

## 5. Application Architecture

```text
Browser
   ↓
HTTP Request
   ↓
PHP Application
   ↓
MySQL/MariaDB
   ↓
Response
```

DVWA demonstrates how insecure application logic interacts with databases, files, and operating-system functions.

---

## 6. SQL Injection

SQL injection occurs when user-controlled data is incorporated into SQL statements unsafely.

Conceptually:

```text
Input
  ↓
Unsafe Query Construction
  ↓
Database
  ↓
Unexpected Query Behavior
```

Testing methodology:

1. Identify database-backed parameters.
2. Establish normal behavior.
3. Review the application's SQL construction.
4. Test harmless input variations.
5. Compare behavior across security levels.
6. Study the secure implementation.

Primary defense:

```text
Parameterized Queries
+
Least Privilege
```

---

## 7. Cross-Site Scripting

DVWA includes exercises demonstrating reflected and stored XSS.

Example harmless lab marker:

```html
<script>alert('XSS')</script>
```

Types:

```text
Reflected XSS
Stored XSS
DOM-Based XSS
```

Key defense:

```text
Context-Aware Encoding
+
Safe DOM APIs
+
HTML Sanitization
```

---

## 8. Command Injection

Command injection occurs when user input is interpreted as operating-system command syntax.

```text
User Input
    ↓
Unsafe Shell Invocation
    ↓
Operating System
```

Review:

- Input validation
- Shell invocation
- Argument construction
- Process privileges
- Error handling

Defensive approach: avoid shell invocation when structured APIs are available.

---

## 9. File Upload Vulnerabilities

DVWA demonstrates weaknesses in upload validation.

Important checks:

```text
File Type
MIME Type
File Content
Filename
Storage Location
Execution Permissions
```

Use harmless test files.

Never expose the intentionally vulnerable upload environment publicly.

---

## 10. File Inclusion

File inclusion vulnerabilities occur when an application uses untrusted input to select files.

Conceptually:

```text
User-Controlled Path
        ↓
File Inclusion Logic
        ↓
Unexpected Resource
```

Related concepts:

```text
Local File Inclusion (LFI)
Remote File Inclusion (RFI)
Path Traversal
```

Defenses:

- Allowlisted file identifiers
- Canonical path validation
- Restricted filesystem permissions
- Disabled unnecessary remote inclusion

---

## 11. CSRF

**Cross-Site Request Forgery** occurs when a browser is induced to perform an unwanted authenticated action.

Defenses include:

```text
CSRF Tokens
SameSite Cookies
Origin Validation
Correct HTTP Method Semantics
```

Compare DVWA's vulnerable and secure implementations.

---

## 12. Authentication Testing

Relevant areas:

```text
Login Validation
Password Policy
Rate Limiting
Session Management
Account Lockout
```

Use only DVWA's lab accounts and approved test credentials.

---

## 13. Burp Suite Integration

```text
Browser
   ↓
Burp Proxy
   ↓
DVWA
   ↓
HTTP Response
```

Recommended workflow:

1. Browse a DVWA vulnerability module.
2. Capture the HTTP request.
3. Send it to Repeater.
4. Modify one input.
5. Compare the response.
6. Change the security level.
7. Review the difference.

---

## 14. Docker Management

Start:

```bash
docker compose up -d
```

Status:

```bash
docker compose ps
```

Logs:

```bash
docker compose logs
```

Stop:

```bash
docker compose down
```

Reset lab data only when intended:

```bash
docker compose down -v
```

**Warning:** `-v` removes associated Compose-managed volumes.

---

## 15. Lab Methodology

```text
Select Vulnerability
       ↓
Understand Normal Behavior
       ↓
Test at Low Security
       ↓
Inspect Application Code
       ↓
Compare Medium / High
       ↓
Review Impossible
       ↓
Document Root Cause
```

---

## 16. Documentation Template

```text
Lab:
DVWA

Vulnerability:
__________

Security Level:
__________

Affected Parameter:
__________

Observed Behavior:
__________

Root Cause:
__________

Secure Implementation:
__________

Lessons Learned:
__________
```

---

## Key Takeaway

DVWA is particularly useful for understanding:

```text
VULNERABLE CODE
      ↓
SECURITY FAILURE
      ↓
ROOT CAUSE
      ↓
SECURE IMPLEMENTATION
```

Focus on comparing security levels rather than simply making a payload work.
