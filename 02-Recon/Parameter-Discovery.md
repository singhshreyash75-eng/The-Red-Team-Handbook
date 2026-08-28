# 🎛️ Parameter Discovery

> Parameter discovery identifies user-controllable inputs accepted by web applications and APIs. These parameters frequently become the entry points for later vulnerability testing.

---

## Overview

Consider:

```text
https://example.com/profile?id=15
```

Here:

```text
id
```

is a parameter.

Other common examples:

```text
?user=
?url=
?file=
?redirect=
?page=
?lang=
?search=
```

Parameters matter because most web vulnerabilities require attacker-controlled input.

---

## Why Parameter Discovery Matters

Different parameter types can become relevant to different vulnerabilities.

| Parameter | Potential testing area |
|---|---|
| `id=` | Access control / IDOR |
| `url=` | SSRF / redirects |
| `redirect=` | Open Redirect |
| `file=` | File handling / inclusion |
| `search=` | Injection / XSS |
| `callback=` | Redirect or integration behavior |
| `user=` | Authorization / application logic |

The parameter name is only a clue, not proof of a vulnerability.

---

## Parameter Locations

Parameters aren't limited to URLs.

They may exist in:

### Query Strings

```http
GET /profile?id=15
```

### POST Forms

```http
POST /login

username=admin&password=test
```

### JSON

```json
{
  "user_id": 15,
  "role": "user"
}
```

### Cookies

```http
Cookie: language=en
```

### Headers

```http
X-Forwarded-For: 192.0.2.10
```

### Path Parameters

```text
/api/users/15
```

Understanding all input locations is critical.

---

## Start with the Application

The highest-quality parameter discovery usually begins by actually using the application.

Interact with:

- Forms
- Search boxes
- Filters
- Sorting
- Profile settings
- Upload functionality
- API requests

Capture traffic using Burp Suite or Caido.

---

## Burp Suite

Useful locations:

```text
Proxy → HTTP History
Target → Site Map
Repeater
```

Look for parameters in:

```text
GET
POST
JSON
Cookies
Headers
```

---

## HTML Forms

Inspect forms such as:

```html
<form method="POST">
    <input name="username">
    <input name="password">
</form>
```

This reveals:

```text
username
password
```

as accepted parameters.

---

## JavaScript

Client-side JavaScript frequently references parameters and API objects.

Example:

```javascript
fetch("/api/user?id=" + userId)
```

Possible discovery:

```text
id
```

JavaScript recon is covered in the next chapter.

---

## Historical URLs

Archived URLs may reveal old parameter names.

Example:

```text
/search?q=test
/product?id=5
/redirect?url=...
```

Useful sources/tools:

```text
gau
waybackurls
```

---

## Arjun

Arjun is designed to discover HTTP parameters.

A typical authorized/lab example:

```bash
arjun -u https://example.com/endpoint
```

Depending on the application, it can test potential parameter names and identify response differences.

Always check the current documentation for available methods and options.

---

## FFUF Concept

FFUF can also fuzz parameter names.

Conceptually:

```text
?FUZZ=value
```

with a parameter-name wordlist.

For example:

```bash
ffuf -w parameters.txt -u "https://example.com/?FUZZ=test"
```

Only use this against authorized systems and with appropriate request rates.

---

## Compare Responses

Suppose:

```text
?random=test
```

returns:

```text
5000 bytes
```

while:

```text
?debug=test
```

returns:

```text
6200 bytes
```

That difference may indicate the application recognized `debug`.

Compare:

- Status
- Length
- Headers
- Redirects
- Body content

---

## Hidden Parameters

Interesting parameters may include:

```text
admin
debug
role
internal
preview
test
```

For example, a parameter may affect functionality even if it never appears in the visible UI.

Do not assume a hidden parameter grants additional privileges; authorization still needs to be tested separately.

---

## JSON APIs

Modern applications frequently send:

```json
{
    "email": "user@example.com",
    "name": "Alice"
}
```

During API testing, identify:

- Required fields
- Optional fields
- IDs
- Boolean flags
- Nested objects

These become relevant later for access-control and mass-assignment testing.

---

## Path Parameters

REST APIs commonly use:

```text
/api/users/15
/api/orders/923
```

The values:

```text
15
923
```

are also user-controlled inputs.

This is particularly important for IDOR testing.

---

## Parameter Discovery Workflow

```text
Browse Application
      ↓
Capture Requests
      ↓
Extract Existing Parameters
      ↓
Inspect JavaScript
      ↓
Inspect Historical URLs
      ↓
Controlled Parameter Discovery
      ↓
Compare Responses
      ↓
Prioritize Inputs
```

---

## Categorize Parameters

A useful notes format:

```text
Endpoint:
/api/profile

Method:
POST

Parameters:
user_id
email
display_name

Authentication:
Required

Interesting:
user_id
```

This makes later vulnerability testing significantly easier.

---

## Red Team Perspective

Parameter discovery answers:

> Where can I influence application behavior?

Once parameters are mapped, later testing can investigate:

```text
Authorization
Injection
SSRF
Redirects
File handling
Business logic
```

Discovery and exploitation should remain separate mental steps.

First understand the input.

Then determine whether the application handles it securely.

---

## Common Beginner Mistakes

- Testing only query-string parameters.
- Ignoring JSON.
- Ignoring cookies.
- Ignoring path IDs.
- Brute-forcing before understanding the application.
- Treating a discovered parameter as a vulnerability.
- Ignoring JavaScript.
- Ignoring historical URLs.

---

## Interview Questions

### What is parameter discovery?

Identifying inputs accepted and processed by an application.

### Where can parameters appear?

URLs, bodies, JSON, cookies, headers, and paths.

### Why is JavaScript useful?

It often reveals API parameters and client-side request structures.

### Why compare response lengths?

Recognized parameters may change application behavior even when status codes remain identical.

---

## Quick Revision

```text
Parameters can exist in:

GET
POST
JSON
Cookies
Headers
Paths

Sources:

Application traffic
HTML
JavaScript
Historical URLs
Arjun
Controlled fuzzing

Discover → Understand → Test
```

---

## References

- OWASP Web Security Testing Guide
- PortSwigger Web Security Academy
- Arjun Documentation
- FFUF Documentation
