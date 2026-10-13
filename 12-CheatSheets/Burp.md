
# Burp Suite — Cheat Sheet

> Quick-reference commands, HTTP concepts, and practical workflows for authorized web-application security testing.

## 1. What is Burp Suite?

**Burp Suite** is an intercepting proxy and web-application security testing platform developed by PortSwigger.

```text
Browser
   ↓
Burp Proxy
   ↓
Web Server
```

It allows:

```text
Intercept
Inspect
Modify
Replay
Compare
Analyze
```

HTTP requests and responses.

---

## 2. Core Tools

| Tool | Purpose |
|---|---|
| Proxy | Intercept HTTP/HTTPS traffic |
| HTTP History | Review captured requests |
| Repeater | Modify and resend requests |
| Intruder | Automate controlled input variations |
| Decoder | Encode/decode data |
| Comparer | Compare responses |
| Sequencer | Analyze token randomness |
| Target | Organize application attack surface |
| Extensions | Add functionality |

---

## 3. Proxy Configuration

Typical local proxy listener:

```text
127.0.0.1:8080
```

Configure a browser to use the proxy.

For HTTPS interception, trust Burp's CA certificate in a dedicated testing browser.

Burp's integrated browser simplifies configuration.

---

## 4. HTTP Request Structure

```http
POST /api/login HTTP/1.1
Host: app.example.test
Content-Type: application/json
Cookie: session=test-session

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

## 5. HTTP Methods

| Method | Typical Purpose |
|---|---|
| GET | Retrieve resource |
| POST | Submit/create data |
| PUT | Replace resource |
| PATCH | Partially modify resource |
| DELETE | Delete resource |
| HEAD | Retrieve response headers |
| OPTIONS | Discover supported options |

Actual behavior depends on the application's implementation.

---

## 6. Status Codes

```text
200 OK
201 Created
204 No Content

301 Permanent Redirect
302 Temporary Redirect

400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
405 Method Not Allowed
429 Too Many Requests

500 Internal Server Error
502 Bad Gateway
503 Service Unavailable
```

Important:

```text
401 = Authentication missing/invalid

403 = Access forbidden
```

Neither status code automatically proves a vulnerability.

---

## 7. Repeater Workflow

```text
Proxy / History
      ↓
Send to Repeater
      ↓
Modify Request
      ↓
Send
      ↓
Analyze Response
```

Useful for:

- Authentication testing
- Authorization testing
- Input validation
- API behavior
- Session handling
- Business logic

---

## 8. Parameter Testing

Example:

```http
GET /profile?id=100 HTTP/1.1
Host: app.example.test
```

Change the object identifier using controlled test accounts.

Check whether the server enforces authorization.

```text
Authenticated User
      ↓
Requested Object
      ↓
Server Authorization Check
      ↓
Allow / Deny
```

This is relevant to IDOR/BOLA testing.

---

## 9. Cookies

Example:

```http
Set-Cookie: session=abc123; Secure; HttpOnly; SameSite=Lax
```

Important attributes:

```text
Secure
HttpOnly
SameSite
Domain
Path
Expires
Max-Age
```

Security checks:

- Secure transmission
- HttpOnly where appropriate
- SameSite configuration
- Session rotation
- Logout invalidation
- Session expiration

---

## 10. Authorization Headers

Bearer token:

```http
Authorization: Bearer <token>
```

Basic authentication:

```http
Authorization: Basic <base64-credentials>
```

Remember:

```text
Base64 != Encryption
```

Protect all authentication material.

---

## 11. JWT

JWT format:

```text
header.payload.signature
```

Review:

```text
Signature verification
Allowed algorithms
Expiration
Issuer
Audience
Key handling
Authorization claims
```

Decoding a JWT does not mean its signature has been bypassed.

---

## 12. Decoder

Common transformations:

```text
URL Encoding
Base64
HTML Encoding
Hex
```

Example:

```text
admin
```

Base64:

```text
YWRtaW4=
```

URL encoding:

```text
space → %20
```

---

## 13. Intruder

Intruder replaces selected request positions with test values.

Example:

```http
GET /products?id=§100§ HTTP/1.1
Host: app.example.test
```

Conceptually:

```text
Base Request
     ↓
Payload Position
     ↓
Test Values
     ↓
Response Comparison
```

Use limited payload sets and appropriate request rates.

---

## 14. Response Comparison

Useful properties:

```text
Status Code
Content Length
Response Body
Headers
Redirect
Response Time
```

Example:

```text
Request A → 403

Request B → 200
```

This is a lead requiring investigation, not automatic proof of authorization bypass.

---

## 15. Authentication Checklist

```text
[ ] Login requires valid credentials
[ ] Session changes after login
[ ] Logout invalidates session
[ ] Password reset tokens expire
[ ] MFA is correctly enforced
[ ] Failed login attempts are throttled
[ ] Session cookies are protected
```

---

## 16. Authorization Checklist

```text
[ ] Object-level authorization
[ ] Function-level authorization
[ ] Role separation
[ ] Tenant isolation
[ ] Server-side enforcement
[ ] No trust in client-controlled role fields
```

---

## 17. API Security

Common areas:

```text
BOLA / IDOR
Broken authentication
Excessive data exposure
Mass assignment
Improper authorization
Rate limiting
Input validation
```

Use controlled test accounts to validate cross-role or cross-object access.

---

## 18. CORS

Important headers:

```http
Access-Control-Allow-Origin: https://app.example.test
Access-Control-Allow-Credentials: true
```

Review:

- Allowed origins
- Credentialed cross-origin requests
- Dynamic origin reflection
- Preflight handling

CORS behavior must be evaluated in browser context.

---

## 19. CSRF

CSRF involves unauthorized state-changing requests triggered through a victim's browser.

Review:

```text
CSRF tokens
SameSite cookies
Origin validation
Referer validation
State-changing methods
```

Do not assume a missing CSRF token automatically proves exploitability.

---

## 20. File Uploads

Review:

```text
File type validation
Content validation
Filename handling
Storage location
Authorization
File size
Execution permissions
```

Use harmless test files.

---

## 21. Useful Response Headers

```http
Content-Security-Policy:
Strict-Transport-Security:
X-Content-Type-Options:
Referrer-Policy:
Permissions-Policy:
```

These can strengthen browser-side protections.

---

## 22. Burp Testing Workflow

```text
Define Scope
    ↓
Configure Proxy
    ↓
Browse Application
    ↓
Review HTTP History
    ↓
Map Endpoints
    ↓
Identify Sensitive Functions
    ↓
Send to Repeater
    ↓
Modify Controlled Inputs
    ↓
Compare Responses
    ↓
Validate Security Impact
```

---

## 23. Reporting Checklist

Record:

```text
Endpoint
HTTP Method
Test Account / Role
Original Request
Modified Request
Relevant Response
Expected Behavior
Actual Behavior
Impact
Remediation
```

Remove real credentials and unnecessary personal information.

---

## Key Takeaway

Burp Suite is most valuable when used to understand:

```text
CLIENT REQUEST
      ↓
SERVER VALIDATION
      ↓
AUTHORIZATION
      ↓
APPLICATION RESPONSE
```

**The core skill is forming and validating security hypotheses through controlled HTTP request manipulation.**

