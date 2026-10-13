
# OWASP Juice Shop

> Modern intentionally vulnerable web application for practicing web security, API testing, authentication, authorization, and business-logic analysis.

## 1. What is OWASP Juice Shop?

**OWASP Juice Shop** is an intentionally insecure modern web application maintained under the OWASP project.

Official repository:

https://github.com/juice-shop/juice-shop

It is designed for security education, CTFs, demonstrations, and testing security tools.

---

## 2. Main Learning Areas

```text
Authentication
Authorization
SQL Injection
Cross-Site Scripting
API Security
Sensitive Data Exposure
Business Logic
Cryptographic Issues
Security Misconfiguration
```

---

## 3. Application Architecture

Conceptually:

```text
Browser
   ↓
Modern Frontend
   ↓
Backend API
   ↓
Application Data
```

Juice Shop provides experience with modern application patterns, including client-side JavaScript and API requests.

---

## 4. Docker Deployment

A commonly used official Docker image is:

```text
bkimminich/juice-shop
```

Run locally:

```bash
docker run --rm \
  -p 127.0.0.1:3000:3000 \
  bkimminich/juice-shop
```

Open:

```text
http://127.0.0.1:3000
```

For reproducible environments, use a reviewed image version rather than relying indefinitely on an unpinned tag.

---

## 5. Verify Deployment

Check containers:

```bash
docker ps
```

HTTP response:

```bash
curl -I http://127.0.0.1:3000
```

Stop the container using Docker or Ctrl+C if running in the foreground.

---

## 6. Why Juice Shop?

Juice Shop is useful because it resembles a modern web application.

It includes:

```text
Frontend JavaScript
API Calls
Authentication
User Accounts
Application State
Shopping Workflows
```

This helps develop practical application-security reasoning.

---

## 7. Browser Developer Tools

Important tabs:

```text
Elements
Console
Network
Sources
Application
```

Investigate:

```text
API Requests
JavaScript Files
Cookies
Local Storage
Application Routing
```

---

## 8. Burp Suite Integration

```text
Browser
   ↓
Burp Proxy
   ↓
Juice Shop
   ↓
API Response
```

Recommended workflow:

1. Browse normally.
2. Capture requests.
3. Identify endpoints.
4. Send requests to Repeater.
5. Change controlled inputs.
6. Observe authorization and validation behavior.

---

## 9. API Security

Important areas:

```text
Object-Level Authorization
Authentication
Function-Level Authorization
Mass Assignment
Rate Limiting
Input Validation
```

Example request:

```http
GET /api/products HTTP/1.1
Host: 127.0.0.1:3000
```

Use application behavior to identify actual API routes.

---

## 10. Authentication

Review:

```text
Login
Registration
Password Reset
Session Management
Token Handling
Logout
```

Important question:

```text
Does the server validate identity
and permissions independently
of client-side controls?
```

---

## 11. Authorization

Conceptually:

```text
User A
   ↓
Requests Object B
   ↓
Server Authorization
   ↓
Allow / Deny
```

Use dedicated test accounts to evaluate access-control boundaries.

---

## 12. XSS

Juice Shop includes XSS-related challenges.

Important contexts:

```text
HTML
JavaScript
DOM
User-Controlled Content
```

Harmless lab proof:

```html
<script>alert('XSS')</script>
```

Defenses:

```text
Context-Aware Encoding
Safe DOM APIs
HTML Sanitization
CSP
```

---

## 13. SQL Injection

SQLi results from unsafe database-query construction.

```text
Untrusted Input
      ↓
Unsafe SQL Query
      ↓
Database
```

Study:

```text
Input Context
Query Structure
Error Handling
Parameterized Queries
```

Avoid destructive operations.

---

## 14. Business Logic

Business logic vulnerabilities are especially valuable learning topics.

Potential issues include:

```text
Incorrect Price Validation
Unexpected Workflow Transitions
Authorization Gaps
Improper Transaction Rules
```

These often require manual testing rather than scanner signatures.

---

## 15. Sensitive Data Exposure

Review:

```text
API Responses
Client-Side State
Error Messages
Configuration
Application Logs
```

Do not assume data visible in frontend JavaScript is necessarily secret.

Evaluate the actual sensitivity and security boundary.

---

## 16. Challenge Tracking

Juice Shop includes challenge-oriented learning functionality.

Use it to track:

```text
Challenge Category
Difficulty
Progress
Concept Learned
```

Avoid relying on complete solution guides before attempting the challenge.

---

## 17. Useful Tools

```text
Burp Suite
Caido
Browser Developer Tools
curl
Katana
FFUF
```

Use low-noise testing settings and keep the application isolated.

---

## 18. DVWA vs Juice Shop

| DVWA | Juice Shop |
|---|---|
| Traditional PHP application | Modern JavaScript-heavy application |
| Explicit security levels | Challenge-based learning |
| Vulnerability-focused modules | Integrated application workflows |
| Useful for fundamentals | Useful for modern web/API security |

Both are complementary.

---

## 19. Recommended Learning Workflow

```text
Deploy Locally
      ↓
Browse Application
      ↓
Inspect HTTP Traffic
      ↓
Map APIs
      ↓
Choose Challenge
      ↓
Form Hypothesis
      ↓
Validate
      ↓
Study Root Cause
      ↓
Document
```

---

## 20. Documentation Template

```text
Challenge:
__________

Category:
__________

Difficulty:
__________

Endpoint:
__________

Input:
__________

Observed Behavior:
__________

Root Cause:
__________

Mitigation:
__________

Lessons Learned:
__________
```

---

## Key Takeaway

OWASP Juice Shop develops:

```text
MODERN WEB APPLICATION
        +
API SECURITY
        +
BUSINESS LOGIC
        +
MANUAL TESTING
```

It is particularly useful for transitioning from basic vulnerability demonstrations to realistic application-security analysis.
