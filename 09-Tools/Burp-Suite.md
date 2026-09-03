# Burp Suite

> Burp Suite fundamentals for authorized web-application security testing, HTTP inspection, request manipulation, and manual analysis.

## What is Burp Suite?

**Burp Suite** is a web-application security testing platform created by PortSwigger.

It operates primarily as an:

```text
Intercepting Proxy
```

between:

```text
Browser
   |
   v
Burp Suite
   |
   v
Web Application
```

This allows HTTP/HTTPS requests and responses to be:

```text
Viewed
Modified
Repeated
Analyzed
Compared
```

---

# Why Use Burp?

A browser normally hides much of the underlying HTTP communication.

Burp exposes it.

Example:

```text
Browser
   |
POST /login
   |
Burp
   |
   v
Server
```

You can inspect:

```text
Method
URL
Headers
Cookies
Parameters
Body
Response
```

This makes Burp one of the core tools for web security testing.

---

# Burp Editions

Common editions include:

```text
Burp Suite Community Edition
Burp Suite Professional
Burp Suite Enterprise / DAST offerings
```

Features differ between editions and evolve over time.

For learning manual testing, Community Edition still provides many core tools.

---

# Core Components

Important Burp tools include:

```text
Proxy
Repeater
Intruder
Decoder
Comparer
Sequencer
Target
Logger / HTTP history
Extensions
```

Professional editions include additional automated scanning capabilities.

---

# Proxy

The **Proxy** sits between the browser and target.

```text
Browser
   |
   | Request
   v
Burp Proxy
   |
   | Request
   v
Server
```

Response:

```text
Server
   |
   | Response
   v
Burp Proxy
   |
   | Response
   v
Browser
```

---

# Intercept

When interception is enabled:

```text
Browser Request
       ↓
      BURP
       ↓
    PAUSED
```

You can:

```text
Inspect
Modify
Forward
Drop
```

the request.

---

# HTTP History

Even with interception disabled, Burp can maintain a history of proxied traffic.

This is extremely useful.

Look for:

```text
Endpoints
Parameters
APIs
Cookies
Status codes
Authentication flows
Interesting responses
```

---

# Configure Browser Proxy

Burp commonly listens locally on a proxy address such as:

```text
127.0.0.1:8080
```

The browser is configured to send HTTP/HTTPS traffic through Burp.

Burp's integrated browser simplifies this setup.

---

# HTTPS Interception

HTTPS traffic is encrypted.

For Burp to inspect HTTPS in an authorized testing browser, the browser needs to trust Burp's local CA certificate.

Conceptually:

```text
Browser
   |
TLS
   |
Burp
   |
TLS
   |
Target
```

Only install testing CA certificates into controlled testing environments.

---

# Target Scope

One of the most important Burp concepts:

```text
Scope
```

Define which hosts are authorized.

Example:

```text
https://app.example.com
```

Then configure tools/views to focus on in-scope traffic.

This reduces:

```text
Noise
Accidental third-party testing
Unrelated requests
```

---

# Target / Site Map

Burp builds a map of observed application content.

Conceptually:

```text
app.example.com
 |
 +-- /
 +-- /login
 +-- /api
 |     |
 |     +-- /users
 |
 +-- /admin
```

This helps understand application structure.

---

# Repeater

**Repeater** is one of the most important Burp tools.

Workflow:

```text
Request
   ↓
Send to Repeater
   ↓
Modify
   ↓
Send
   ↓
Observe Response
   ↓
Modify Again
```

It is ideal for manual testing.

---

# Send to Repeater

From an intercepted/history request:

```text
Right Click
    ↓
Send to Repeater
```

Then modify:

```text
Parameters
Headers
Cookies
Body
HTTP method
```

and resend.

---

# Repeater Example

Original authorized-lab request:

```http
GET /profile?id=100 HTTP/1.1
Host: app.example.test
```

In Repeater, change:

```text
id=100
```

to another test value within the authorized environment and observe whether authorization is enforced correctly.

The security question is:

```text
Does the server validate
that the current user is
authorized for the requested object?
```

This is useful for identifying access-control issues such as IDOR/BOLA in test environments.

---

# Server-Side Validation

Never assume:

```text
Button hidden in UI
=
Action forbidden
```

Burp allows direct HTTP requests.

Therefore:

```text
Client-side restriction
      !=
Server-side authorization
```

Security controls must be enforced by the server.

---

# Intruder

**Intruder** automates repeated requests with changing inputs.

Conceptually:

```text
Base Request
      ↓
Payload Positions
      ↓
Payload List
      ↓
Many Requests
      ↓
Compare Responses
```

Use rate limits and explicit authorization because automated requests can create significant traffic.

---

# Appropriate Intruder Uses

In a controlled lab, Intruder can help test:

```text
Input variations
Parameter behavior
Enumeration resistance
Fuzzing
Request permutations
```

Do not use it for unauthorized credential attacks or uncontrolled high-volume traffic.

---

# Intruder Positions

Example request:

```http
GET /item?id=§100§ HTTP/1.1
```

The marked value becomes a payload position.

Burp substitutes configured test values.

---

# Response Analysis

Useful comparison properties:

```text
Status Code
Response Length
Headers
Body
Response Time
Redirect Location
```

Different responses can reveal different application behavior.

---

# Decoder

**Decoder** helps transform data.

Common formats include:

```text
URL encoding
Base64
Hex
HTML encoding
```

Example:

```text
admin
```

Base64:

```text
YWRtaW4=
```

Remember:

```text
Encoding != Encryption
```

---

# URL Encoding

Example:

```text
space
```

can become:

```text
%20
```

Special characters frequently require URL encoding in HTTP requests.

---

# Comparer

**Comparer** compares two pieces of data.

Useful for:

```text
Responses
Tokens
Headers
Error messages
```

Example:

```text
Normal Response
      vs
Modified Request Response
```

Differences can reveal application logic.

---

# Sequencer

**Sequencer** analyzes the statistical quality of tokens.

Potential examples:

```text
Session tokens
CSRF tokens
Password-reset tokens
```

It helps assess whether values appear sufficiently unpredictable.

---

# Cookies

Burp makes cookies easy to inspect.

Example:

```http
Cookie: session=abc123
```

Review attributes such as:

```text
Secure
HttpOnly
SameSite
Domain
Path
Expires / Max-Age
```

---

# Secure Flag

```text
Secure
```

instructs browsers to send the cookie only over secure HTTPS connections.

---

# HttpOnly

```text
HttpOnly
```

restricts JavaScript access to the cookie.

This can reduce certain cookie-theft scenarios involving XSS, though it does not fix XSS itself.

---

# SameSite

```text
SameSite
```

controls aspects of cross-site cookie sending.

Common values include:

```text
Strict
Lax
None
```

Correct configuration depends on application requirements.

---

# Authentication Testing

Burp is particularly useful for understanding:

```text
Login flow
Logout flow
Password reset
MFA flow
Session creation
Session invalidation
Authorization
```

Map the complete authentication lifecycle.

---

# Authorization Testing

For each sensitive request ask:

```text
Who is the current user?

What object is requested?

What action is attempted?

Does the server enforce permission?
```

Test roles only with authorized test accounts.

---

# HTTP Methods

Common methods:

```text
GET
POST
PUT
PATCH
DELETE
OPTIONS
HEAD
```

Different methods can expose different application behavior.

Burp Repeater makes method testing straightforward.

---

# Headers

Important headers include:

```text
Host
Authorization
Cookie
Content-Type
Origin
Referer
User-Agent
X-Forwarded-For
```

Headers can influence application routing, authentication, content parsing, and security controls.

---

# Authorization Header

APIs frequently use:

```http
Authorization: Bearer <token>
```

Treat bearer tokens as credentials.

Anyone possessing a valid bearer token may potentially exercise the permissions represented by that token.

---

# JSON APIs

Example:

```http
POST /api/profile HTTP/1.1
Content-Type: application/json

{
  "name": "Alice"
}
```

Burp Repeater makes it easy to modify JSON fields and observe server-side validation.

---

# Content-Type

Applications may accept:

```text
application/json
application/x-www-form-urlencoded
multipart/form-data
text/xml
```

Understanding parsing behavior is important when testing input validation.

---

# File Uploads

Burp can inspect multipart requests.

Example structure:

```http
Content-Type: multipart/form-data
```

Security review should examine:

```text
File type validation
File size
Storage location
Authorization
Filename handling
Content processing
```

Use harmless test files in authorized environments.

---

# Proxy Match and Replace

Burp can modify traffic automatically using match/replace-style rules.

This can be useful for controlled testing of:

```text
Headers
Cookies
Request values
```

Use carefully to avoid unintentionally changing unrelated traffic.

---

# Extensions

Burp supports extensions through:

```text
BApp Store
Montoya API / extension APIs
```

Extensions can add functionality.

Treat third-party extensions as software dependencies and review their trustworthiness before installation.

---

# Burp + Recon Tools

Typical workflow:

```text
Subfinder / Amass
        ↓
httpx
        ↓
Interesting Application
        ↓
Burp Suite
        ↓
Manual Testing
```

Recon finds applications.

Burp helps understand them deeply.

---

# Burp + FFUF

Another workflow:

```text
Application
     ↓
FFUF
     ↓
Interesting Endpoint
     ↓
Burp Repeater
     ↓
Manual Validation
```

Automated discovery and manual testing complement each other.

---

# Burp + Caido

Burp and Caido overlap in the intercepting-proxy/web-testing space.

Both can help:

```text
Capture requests
Modify requests
Replay requests
Analyze applications
```

The best tool depends on workflow and requirements.

---

# Common Web Testing Areas

Burp is useful when examining:

```text
Authentication
Authorization
Session management
Input validation
API security
File uploads
Business logic
CSRF
CORS
Security headers
Error handling
```

---

# Manual Testing Mindset

Do not treat Burp as:

```text
Press Scan
   ↓
Pentest Done
```

A stronger workflow is:

```text
Understand Application
        ↓
Map Functionality
        ↓
Identify Trust Boundaries
        ↓
Modify Requests
        ↓
Compare Responses
        ↓
Test Hypothesis
        ↓
Validate Impact
```

---

# Burp Workflow

```text
1. Configure proxy

2. Define scope

3. Browse application normally

4. Review HTTP history

5. Build site map

6. Identify sensitive endpoints

7. Send requests to Repeater

8. Modify one variable at a time

9. Compare responses

10. Document reproducible findings
```

---

# Important Rule

A response difference is not automatically a vulnerability.

Example:

```text
200 vs 403
```

or:

```text
Response length changed
```

is only evidence of different behavior.

You still need to understand:

```text
Why did it change?
Can security boundaries be crossed?
What is the impact?
```

---

# Evidence Collection

For confirmed findings, preserve:

```text
Original request
Modified request
Relevant response
User/role context
Expected behavior
Actual behavior
Impact
```

Avoid collecting unnecessary sensitive user data.

---

# Key Takeaway

Burp Suite's core mental model is:

```text
BROWSER
   ↓
REQUEST
   ↓
BURP
   ↓
INSPECT / MODIFY
   ↓
SERVER
   ↓
RESPONSE
   ↓
BURP
   ↓
ANALYZE
```

The most valuable component for learning manual web security is often:

```text
REPEATER
```

because it teaches the fundamental skill:

```text
Take a legitimate request
        ↓
Change one assumption
        ↓
Observe server behavior
        ↓
Determine whether the
server enforces the security boundary
```
