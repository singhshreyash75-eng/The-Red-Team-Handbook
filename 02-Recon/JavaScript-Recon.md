# 📜 JavaScript Reconnaissance

> Modern web applications often expose significant portions of their client-side architecture through JavaScript. Reviewing JavaScript can reveal endpoints, routes, parameters, integrations, and functionality that are not obvious from the visible interface.

---

## Overview

Modern applications commonly use frameworks such as:

```text
React
Angular
Vue
Next.js
```

A page may download large JavaScript bundles containing application logic.

Example:

```text
/assets/app.js
/static/main.js
/_next/static/...
```

These files can provide valuable reconnaissance information.

---

## Why JavaScript Recon Matters

JavaScript may reveal:

- API endpoints
- Hidden routes
- Parameter names
- WebSocket endpoints
- GraphQL endpoints
- Feature flags
- Environment references
- Third-party services
- Client-side validation logic

It can also reveal secrets accidentally embedded in public client-side code, although not every API key or identifier is sensitive.

---

## Typical Workflow

```text
Application
     ↓
Collect JavaScript URLs
     ↓
Download / Inspect
     ↓
Beautify if Necessary
     ↓
Search for Interesting Patterns
     ↓
Extract Endpoints
     ↓
Validate In-Scope Findings
```

---

## Finding JavaScript Files

HTML commonly contains:

```html
<script src="/assets/app.js"></script>
```

Browser Developer Tools can show loaded scripts.

Useful locations:

```text
DevTools → Sources
DevTools → Network → JS
```

Burp's site map also records JavaScript requested while browsing.

---

## Crawling with Katana

For an authorized application:

```bash
katana -u https://example.com
```

Filter collected output afterward to identify JavaScript resources.

Tool options change, so consult current documentation for JavaScript-specific extraction features.

---

## Manual Collection

Retrieve a known script:

```bash
curl https://example.com/assets/app.js
```

or:

```bash
wget https://example.com/assets/app.js
```

Large minified bundles are usually easier to inspect after formatting/beautification.

---

## Beautification

Minified code may look like:

```javascript
function a(b){return fetch("/api/u/"+b)}
```

After formatting:

```javascript
function getUser(id) {
    return fetch("/api/u/" + id);
}
```

Now the application behavior is easier to understand.

Browser developer tools can often pretty-print minified scripts directly.

---

## Search for API Endpoints

Example:

```javascript
fetch("/api/v1/users")
```

Possible endpoint:

```text
/api/v1/users
```

Another example:

```javascript
axios.post("/api/login", data)
```

Possible endpoint:

```text
/api/login
```

---

## Useful Search Terms

When reviewing code, look for strings such as:

```text
/api/
/admin/
/graphql
/websocket
/upload
/login
/auth
/token
/debug
/internal
```

These are starting points for understanding application architecture.

---

## Grep

For locally downloaded JavaScript:

```bash
grep -Ri "/api/" .
```

Search for GraphQL references:

```bash
grep -Ri "graphql" .
```

Search for WebSockets:

```bash
grep -Ri "websocket\|wss://" .
```

Search results should always be manually interpreted.

---

## Parameters

Example:

```javascript
fetch("/api/profile?user_id=" + id)
```

Reveals:

```text
Endpoint:
/api/profile

Parameter:
user_id
```

This can later inform authorization testing.

---

## GraphQL

Look for:

```text
/graphql
```

or code containing:

```text
query
mutation
```

A GraphQL endpoint should be documented for later API-security testing.

---

## WebSockets

Look for:

```text
ws://
wss://
```

Example:

```javascript
new WebSocket("wss://chat.example.com/socket")
```

This exposes another application communication channel.

---

## Client-Side Routes

Single-page applications may define routes such as:

```javascript
/admin
/settings
/internal
/billing
```

A route existing in JavaScript doesn't mean the user is authorized to access it, but it may reveal functionality worth reviewing.

---

## Environment References

Bundles may contain strings such as:

```text
production
staging
development
```

or hostnames such as:

```text
api-dev.example.com
api-stage.example.com
```

These can expand the asset map if they are within scope.

---

## API Keys and Secrets

You may encounter strings resembling:

```text
API_KEY
CLIENT_ID
TOKEN
SECRET
```

Important distinction:

### Public identifiers

Some services intentionally expose client-side keys or IDs.

### Sensitive credentials

Private keys, backend secrets, and privileged tokens generally should not appear in public JavaScript.

Do not assume every string named `API_KEY` is a vulnerability.

Determine:

- What service uses it?
- Is it intended to be public?
- What permissions does it provide?
- Is the associated asset within scope?

---

## Source Maps

Applications may publish:

```text
app.js.map
main.js.map
```

Source maps can expose:

- Original source filenames
- Directory structures
- More readable code
- Original function names

Example:

```text
app.min.js
      ↓
app.min.js.map
      ↓
Original Source Structure
```

They can make application analysis much easier.

---

## Framework-Specific Assets

### Next.js

Look for resources under:

```text
/_next/
```

### React

Common files:

```text
main.js
bundle.js
runtime.js
```

### Angular

Common files:

```text
main.js
polyfills.js
runtime.js
```

The exact structure varies by build system and version.

---

## Third-Party Services

JavaScript frequently reveals integrations with:

- Analytics
- Payment providers
- Authentication services
- CDNs
- Error monitoring
- Cloud storage

These references help map application dependencies.

Do not actively test third-party infrastructure unless it is explicitly included in scope.

---

## Browser DevTools Workflow

```text
Open Application
      ↓
Developer Tools
      ↓
Network
      ↓
Filter: JS
      ↓
Inspect Scripts
      ↓
Sources
      ↓
Search
```

Global search is especially useful for:

```text
/api/
token
graphql
admin
```

---

## Burp Workflow

```text
Browse Application Through Burp
        ↓
Target Site Map
        ↓
Find .js Files
        ↓
Inspect Response
        ↓
Search Interesting Strings
        ↓
Document Endpoints
```

---

## Build an Endpoint List

Example:

```text
/api/login
/api/profile
/api/users
/api/upload
/graphql
/socket
```

Then categorize:

```text
Authentication
User Data
Upload
GraphQL
WebSocket
```

This becomes the starting point for deeper API testing.

---

## Red Team Perspective

Don't approach JavaScript recon as:

> Search for `password` and hope for secrets.

A better question is:

> What does this code tell me about how the application works?

Useful intelligence includes:

```text
Routes
Endpoints
Parameters
Roles
Features
Integrations
Authentication flow
Environment names
```

Understanding application architecture often provides more value than finding a random hardcoded string.

---

## Common Beginner Mistakes

- Looking only for secrets.
- Ignoring source maps.
- Ignoring minified bundles because they're hard to read.
- Assuming every API key is sensitive.
- Testing third-party endpoints without checking scope.
- Collecting endpoints without validating them.
- Ignoring client-side routes.
- Forgetting WebSocket and GraphQL references.

---

## Interview Questions

### Why inspect JavaScript during recon?

It can reveal application routes, endpoints, parameters, and architecture.

### What is a source map?

A mapping between transformed/minified JavaScript and its original source.

### Is every client-side API key a vulnerability?

No. Some keys are intentionally public; security impact depends on permissions and intended usage.

### Why search for `/api/`?

It frequently identifies backend API routes used by the frontend.

---

## Quick Revision

```text
Collect JS
   ↓
Beautify
   ↓
Search
   ↓
Extract

Look for:

/api/
/graphql
ws://
wss://
admin
auth
upload
token
staging

Also check:

Source Maps
Routes
Parameters
Integrations
```

---

## Practice

Useful environments:

- PortSwigger Web Security Academy
- OWASP Juice Shop
- Your own React/Next.js applications
- TryHackMe web-security labs
- Hack The Box Academy

---

## References

- OWASP Web Security Testing Guide
- MDN JavaScript Documentation
- ProjectDiscovery Katana Documentation
- PortSwigger Web Security Academy
- OWASP Juice Shop
