# Caido

> Caido fundamentals for authorized web-application and API security testing.

## What is Caido?

**Caido** is a modern web-security testing toolkit centered around intercepting, inspecting, modifying, and replaying HTTP traffic.

Conceptually:

```text
Browser
   |
HTTP / HTTPS
   |
   v
 Caido
   |
   v
Web Application
```

It occupies a similar general tool category to:

```text
Burp Suite
OWASP ZAP
```

---

# Core Purpose

Caido allows a tester to:

```text
Capture HTTP requests
Inspect responses
Modify parameters
Replay requests
Search traffic
Organize targets
Analyze application behavior
```

The fundamental workflow is:

```text
Application Request
        ↓
Capture
        ↓
Inspect
        ↓
Modify
        ↓
Replay
        ↓
Compare Response
```

---

# HTTP Proxy

Caido acts as an intercepting proxy.

Without proxy:

```text
Browser
   |
   v
Server
```

With Caido:

```text
Browser
   |
   v
Caido
   |
   v
Server
```

This gives visibility into application traffic.

---

# HTTP Request Anatomy

Example:

```http
POST /api/login HTTP/1.1
Host: app.example.test
Content-Type: application/json
Cookie: session=abc123

{
  "username": "alice",
  "password": "example"
}
```

Important components:

```text
Method
Path
Headers
Cookies
Parameters
Body
```

---

# HTTP Response

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "status": "success"
}
```

Important response properties:

```text
Status Code
Headers
Cookies
Body
Length
Timing
```

---

# Intercepting Requests

With interception enabled:

```text
Browser
   ↓
Request
   ↓
CAIDO
   ↓
Paused
```

The tester can inspect or modify the request before forwarding it.

This is useful for understanding what the client actually sends.

---

# HTTPS

HTTPS traffic is encrypted.

To inspect HTTPS traffic in a controlled testing browser, the proxy normally uses a locally trusted testing CA certificate.

Conceptually:

```text
Browser
   |
 TLS
   |
 Caido
   |
 TLS
   |
Target
```

Only install testing certificates in environments you control.

---

# Scope

Always define authorized targets.

Example:

```text
app.example.test
api.example.test
```

Conceptually:

```text
Traffic
  |
  +--> In Scope  -> Analyze
  |
  +--> Out Scope -> Ignore
```

This prevents accidental interaction with unrelated services.

---

# HTTP History

Captured requests provide an application map.

Look for:

```text
API endpoints
Authentication requests
Cookies
Tokens
Parameters
Hidden functionality
Admin routes
File uploads
GraphQL endpoints
```

Example:

```text
/login
/api/users
/api/profile
/api/orders
/admin
```

---

# Replay

One of the most important web-testing capabilities is request replay.

Conceptually:

```text
Captured Request
       ↓
Replay
       ↓
Modify Parameter
       ↓
Send Again
       ↓
Compare Response
```

This is similar conceptually to Burp Repeater.

---

# Example Replay

Original authorized-lab request:

```http
GET /api/profile?id=100 HTTP/1.1
Host: app.example.test
```

Modify:

```text
id=100
```

to another controlled test value.

Then determine whether the server correctly enforces object-level authorization.

The important question is:

```text
Does the server authorize
the requested object?
```

---

# Client vs Server Security

A recurring web-security principle:

```text
Hidden button
Disabled field
JavaScript validation
```

are client-side controls.

They do not replace:

```text
Server-side authorization
Server-side validation
```

An intercepting proxy demonstrates this because requests can be changed independently of the browser UI.

---

# Authentication Testing

Capture and understand:

```text
Login
Logout
Registration
Password reset
MFA
Session creation
Token refresh
```

Map:

```text
Credentials
     ↓
Authentication
     ↓
Session / Token
     ↓
Authenticated Requests
```

---

# Session Analysis

Applications may use:

```text
Cookies
Bearer tokens
JWTs
API keys
```

Example:

```http
Authorization: Bearer <token>
```

Treat session and bearer tokens as credentials.

---

# Cookies

Inspect attributes such as:

```text
Secure
HttpOnly
SameSite
Domain
Path
Expires
```

Example:

```http
Set-Cookie: session=abc;
Secure;
HttpOnly;
SameSite=Lax
```

---

# API Testing

Caido is particularly useful for APIs because raw requests are easy to inspect.

Example:

```http
PATCH /api/users/100 HTTP/1.1
Content-Type: application/json

{
  "email": "alice@example.test"
}
```

Test:

```text
Authentication
Authorization
Input validation
Object ownership
Unexpected parameters
HTTP methods
```

only within the authorized environment.

---

# JSON

Modern applications frequently use:

```text
application/json
```

Example:

```json
{
  "username": "alice",
  "role": "user"
}
```

The server must not trust security-sensitive client-controlled fields without authorization checks.

---

# URL Parameters

Example:

```text
/profile?id=100
```

Parameters may exist in:

```text
Query string
Path
Headers
Cookies
Request body
```

Inspect all input locations.

---

# Status Codes

Common HTTP status codes:

```text
200 OK
201 Created
204 No Content

301/302 Redirect

400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found

500 Internal Server Error
```

Status codes provide clues but are not proof of a vulnerability.

---

# Response Comparison

When changing input, compare:

```text
Status
Length
Body
Headers
Timing
Redirect
```

Example:

```text
Request A -> 403

Request B -> 200
```

Then determine:

```text
Why?
Did authorization change?
Was different content returned?
```

---

# Search and Filtering

Large applications generate large amounts of traffic.

Useful filters include:

```text
Host
Method
Status
Path
Content type
```

Conceptually:

```text
All Traffic
    ↓
Filter
    ↓
Interesting Requests
```

---

# Caido vs Burp Suite

Both can provide:

```text
Intercepting proxy
HTTP history
Request replay
Request modification
Web/API testing workflows
```

Conceptually:

```text
Browser
   |
   +--> Burp
   |
   +--> Caido
          |
          v
      Web Server
```

The better choice depends on workflow and feature requirements.

---

# Caido + FFUF

Recon workflow:

```text
FFUF
 ↓
Interesting Endpoint
 ↓
Caido
 ↓
Manual Request Analysis
```

Automated discovery finds candidates.

Caido validates application behavior.

---

# Caido + httpx

```text
Subdomains
    ↓
httpx
    ↓
Live Applications
    ↓
Caido
    ↓
Manual Testing
```

---

# Caido + Nuclei

```text
Nuclei Finding
      ↓
Caido
      ↓
Manual Validation
```

Automated findings should not be reported blindly.

---

# Testing Methodology

Use:

```text
1. Define scope

2. Configure proxy

3. Browse normally

4. Capture requests

5. Map endpoints

6. Identify authentication boundaries

7. Identify authorization boundaries

8. Replay interesting requests

9. Change one variable at a time

10. Compare responses

11. Validate impact

12. Record evidence
```

---

# Key Security Questions

For every sensitive request ask:

```text
Who is making the request?

What resource is requested?

What action is being performed?

Which parameter identifies the resource?

Does the server enforce authorization?

Can unexpected input change behavior?
```

---

# Evidence

For confirmed findings preserve:

```text
Original Request
Modified Request
Relevant Response
Account/Role
Expected Behavior
Actual Behavior
Impact
```

Avoid unnecessary collection of sensitive information.

---

# Key Takeaway

Caido's fundamental workflow is:

```text
CAPTURE
   ↓
INSPECT
   ↓
MODIFY
   ↓
REPLAY
   ↓
COMPARE
   ↓
UNDERSTAND
```

The tool is valuable because it exposes the real security boundary:

```text
HTTP Request
      ↓
SERVER
      ↓
Authorization + Validation
```

Never assume the browser UI represents everything the server will accept.
