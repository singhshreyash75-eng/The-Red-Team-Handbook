
# XSS — Cross-Site Scripting Cheat Sheet

> Practical XSS fundamentals, vulnerability identification, safe proof-of-concept testing, browser execution contexts, and defensive techniques.

## 1. What is XSS?

**XSS = Cross-Site Scripting**

XSS is a web vulnerability that occurs when attacker-controlled input is interpreted as executable content by a victim's browser.

Conceptually:

```text
User-Controlled Input
        ↓
Web Application
        ↓
Unsafe HTML / JavaScript Context
        ↓
Browser Executes Script
```

Potential impact:

- Session compromise
- Unauthorized actions
- Sensitive information disclosure
- Phishing and interface manipulation
- Application behavior modification

The actual impact depends on the application's security controls and execution context.

---

## 2. Types of XSS

Three major categories:

```text
Reflected XSS
Stored XSS
DOM-Based XSS
```

### Reflected XSS

Input is immediately reflected into an HTTP response without appropriate protection.

```text
User Input
    ↓
Server
    ↓
Response
    ↓
Browser Execution
```

Example vulnerable behavior:

```html
<p>Search results for: USER_INPUT</p>
```

If input is inserted without safe encoding, the browser may interpret it as HTML.

### Stored XSS

Malicious content is stored by the application and later displayed to other users.

```text
Submitted Input
      ↓
Database
      ↓
Another User Loads Page
      ↓
Unsafe Rendering
      ↓
Script Execution
```

Common locations:

```text
Comments
User profiles
Forum posts
Messages
Product reviews
Support tickets
```

### DOM-Based XSS

Occurs when client-side JavaScript handles untrusted data unsafely.

```text
Untrusted Browser Data
        ↓
JavaScript
        ↓
Unsafe DOM Operation
        ↓
Script Execution
```

DOM XSS may occur without the server directly reflecting the malicious input into HTML.

---

## 3. XSS Testing Contexts

XSS payload behavior depends heavily on the injection context.

Important contexts:

```text
HTML Body
HTML Attribute
JavaScript String
URL
DOM
HTML Template
```

The same input may behave differently in each context.

---

## 4. Basic HTML Injection

Harmless HTML marker for an authorized lab:

```html
<b>XSS-TEST</b>
```

If the application renders this as bold text instead of displaying the literal markup, HTML interpretation is occurring.

Important:

```text
HTML Injection
     !=
Confirmed JavaScript Execution
```

Further context-specific validation is required.

---

## 5. Basic XSS Proof of Concept

A classic harmless lab demonstration:

```html
<script>alert('XSS')</script>
```

This attempts to execute JavaScript in the browser.

If executed in the intended application context, it demonstrates a script-execution issue.

However, failure does not prove the application is safe.

---

## 6. HTML Event Handler Context

Browsers support event-handler attributes.

Example for an isolated lab:

```html
<img src="invalid-image" onerror="alert('XSS')">
```

Conceptually:

```text
HTML Element
     ↓
Browser Event
     ↓
JavaScript Handler
```

Whether this executes depends on sanitization, CSP, and the surrounding context.

---

## 7. JavaScript Execution Context

Suppose a page constructs:

```javascript
const message = "USER_INPUT";
```

Unsafe interpolation can allow input to affect JavaScript syntax.

The correct defense is to serialize data safely and avoid inserting untrusted strings directly into executable code.

Prefer:

```javascript
const message = JSON.parse(
  document.getElementById("data").textContent
);
```

with data generated using a proper JSON serializer and safely embedded in a non-executable context.

---

## 8. HTML Attribute Context

Example:

```html
<input value="USER_INPUT">
```

If input is placed inside an HTML attribute, quotation marks and special characters require context-appropriate encoding.

Secure rendering must ensure input remains an attribute value rather than becoming markup.

---

## 9. URL Context

Example:

```html
<a href="USER_INPUT">Open</a>
```

Security considerations:

```text
Allowed URL schemes
Destination validation
Attribute encoding
Untrusted redirects
```

Do not allow arbitrary executable URL schemes in untrusted links.

---

## 10. DOM Sources

A **source** is where untrusted data enters JavaScript.

Common examples:

```javascript
location.href
location.search
location.hash
document.referrer
window.name
```

Other sources include messages, storage, and API responses.

---

## 11. DOM Sinks

A **sink** is an operation that uses data in a potentially dangerous context.

Examples:

```javascript
element.innerHTML
document.write()
eval()
```

Conceptually:

```text
SOURCE
   ↓
Untrusted Data
   ↓
SINK
   ↓
Potential Execution
```

Not every use of a sink is automatically vulnerable; the data flow and sanitization matter.

---

## 12. innerHTML vs textContent

Potentially unsafe:

```javascript
element.innerHTML = userInput;
```

Safer for displaying plain text:

```javascript
element.textContent = userInput;
```

Difference:

```text
innerHTML
=
Interprets HTML markup
```

```text
textContent
=
Treats input as text
```

Use `textContent` when HTML interpretation is unnecessary.

---

## 13. URL Encoding

Common encoded characters:

| Character | URL Encoding |
|---|---|
| Space | `%20` |
| `<` | `%3C` |
| `>` | `%3E` |
| `"` | `%22` |
| `'` | `%27` |
| `&` | `%26` |

Encoding alone is not a universal XSS defense.

Different contexts require different handling.

---

## 14. HTML Encoding

Important characters:

```text
<  → &lt;
>  → &gt;
"  → &quot;
'  → &#39;
&  → &amp;
```

For example:

```html
&lt;script&gt;alert('XSS')&lt;/script&gt;
```

is displayed as text in an HTML text context rather than interpreted as a script element.

Use trusted context-aware encoders rather than manually replacing a few characters.

---

## 15. XSS and Cookies

Cookies may contain session identifiers.

Important cookie attributes:

```text
HttpOnly
Secure
SameSite
```

### HttpOnly

Prevents ordinary JavaScript access to the cookie value.

### Secure

Restricts cookie transmission to secure HTTPS connections.

### SameSite

Controls certain cross-site cookie-sending behaviors.

Important:

```text
HttpOnly
     !=
Complete XSS Protection
```

XSS can still perform actions in the victim's browser even when cookies are HttpOnly.

---

## 16. Content Security Policy

**CSP = Content Security Policy**

CSP helps restrict which content and scripts a browser may execute.

Example:

```http
Content-Security-Policy: default-src 'self'; script-src 'self'
```

A stronger application-specific policy may use script nonces or hashes.

Important:

```text
CSP
=
Defense in Depth
```

It does not replace proper output encoding and safe DOM operations.

---

## 17. CSP Nonces

A nonce is a fresh, unpredictable value associated with approved script elements.

Conceptually:

```text
Server Generates Nonce
        ↓
CSP Header
        +
Approved Script
        ↓
Browser Validates
```

Nonce values should be generated securely for each response.

---

## 18. HTML Sanitization

Some applications intentionally accept rich HTML.

Examples:

```text
Rich-text editors
Blog posts
Forum comments
Formatted messages
```

In such cases, escaping everything may not meet functional requirements.

Use a maintained HTML sanitizer such as DOMPurify in appropriate browser-side workflows.

Example:

```javascript
const clean = DOMPurify.sanitize(userInput);
element.innerHTML = clean;
```

Sanitizer configuration and version must be appropriate for the application's threat model.

---

## 19. XSS vs CSRF

### XSS

```text
Untrusted Input
      ↓
Browser Script Execution
```

### CSRF

```text
Victim Browser
      ↓
Unwanted Authenticated Request
```

Important:

```text
XSS != CSRF
```

But XSS can sometimes undermine CSRF defenses by executing code in the application's origin.

---

## 20. XSS vs HTML Injection

HTML injection:

```text
Untrusted HTML
      ↓
Page Structure Modification
```

XSS:

```text
Untrusted Input
      ↓
JavaScript Execution
```

HTML injection may be security-relevant even without JavaScript execution.

---

## 21. XSS Testing with Burp Suite

Authorized testing workflow:

```text
Browser
   ↓
Burp Proxy
   ↓
Capture Request
   ↓
Send to Repeater
   ↓
Modify Test Parameter
   ↓
Observe Response
   ↓
Inspect Browser Rendering
```

Example:

```http
GET /search?q=XSS-TEST HTTP/1.1
Host: app.example.test
```

First determine whether the marker is reflected and where it appears.

---

## 22. XSS Testing with Caido

Similar workflow:

```text
Capture
   ↓
Replay
   ↓
Modify Input
   ↓
Inspect Response
   ↓
Verify Browser Behavior
```

Use isolated test accounts and harmless markers.

---

## 23. Reflected XSS Testing Workflow

```text
Identify Input Parameter
        ↓
Submit Unique Marker
        ↓
Find Marker in Response
        ↓
Determine HTML/JS Context
        ↓
Check Encoding
        ↓
Use Minimal Safe PoC
        ↓
Confirm Execution
```

Do not assume reflection alone proves XSS.

---

## 24. Stored XSS Testing Workflow

```text
Identify Stored Input
        ↓
Submit Harmless Marker
        ↓
Save
        ↓
Load Relevant Page
        ↓
Inspect Rendering
        ↓
Verify Safe Execution Behavior
```

Use only test accounts and test content.

Avoid involving real users.

---

## 25. DOM XSS Testing Workflow

```text
Identify Data Source
        ↓
Trace JavaScript Data Flow
        ↓
Identify Sink
        ↓
Inspect Transformations
        ↓
Test Harmless Input
        ↓
Verify Browser Execution
```

Browser developer tools are particularly useful for DOM-based issues.

---

## 26. Browser Developer Tools

Useful tabs:

```text
Elements
Console
Network
Sources
Application
```

### Elements

Inspect the rendered DOM.

### Console

Observe JavaScript errors and test behavior.

### Network

Inspect requests and responses.

### Sources

Debug client-side JavaScript.

### Application

Inspect cookies, storage, and application state.

---

## 27. False Positives

Common causes:

```text
Input reflected but encoded
Payload displayed as text
CSP blocks execution
HTML sanitized correctly
Input placed in a non-executable context
Browser behavior misunderstood
```

A vulnerability should be confirmed through reproducible evidence.

---

## 28. False Negatives

A single failed payload does not prove safety.

Possible reasons:

```text
Wrong context
Context-specific encoding
CSP
Sanitization
Different rendering path
Client-side transformation
```

The objective is to understand the actual data flow, not blindly try more payloads.

---

## 29. Framework Security

Modern frameworks often provide automatic escaping.

Examples:

```text
React
Vue
Angular
Django templates
Jinja2 with autoescaping
```

However, unsafe APIs, explicit HTML insertion, or bypassed framework protections can reintroduce XSS risks.

Never assume framework usage alone guarantees safety.

---

## 30. React Example

Safe plain-text rendering:

```jsx
function Message({ value }) {
  return <p>{value}</p>;
}
```

React normally escapes text inserted this way.

Potentially dangerous if data is untrusted:

```jsx
<div dangerouslySetInnerHTML={{ __html: value }} />
```

Only use raw HTML rendering with appropriate sanitization and a justified requirement.

---

## 31. Server-Side Output Encoding

The correct encoding depends on context:

```text
HTML text
HTML attribute
JavaScript
CSS
URL
```

A generic replacement function is not sufficient for all contexts.

Prefer framework-provided escaping and dedicated encoders.

---

## 32. XSS Security Checklist

```text
[ ] Identify untrusted input
[ ] Trace input to output
[ ] Determine rendering context
[ ] Verify context-aware encoding
[ ] Avoid dangerous DOM sinks
[ ] Sanitize permitted HTML
[ ] Review CSP
[ ] Use HttpOnly/Secure cookies
[ ] Test reflected input
[ ] Test stored content
[ ] Test DOM data flows
[ ] Validate remediation
```

---

## 33. Reporting Template

```text
Finding:
Cross-Site Scripting

Type:
Reflected / Stored / DOM-Based

Affected Endpoint:
__________

Input Parameter:
__________

Execution Context:
__________

Test Account:
__________

Proof of Concept:
Harmless local execution marker

Observed Behavior:
__________

Impact:
__________

Root Cause:
__________

Recommended Fix:
__________
```

Include only the evidence necessary to establish the issue.

---

## 34. Defensive Priorities

Recommended order:

```text
1. Avoid unsafe DOM APIs

2. Use context-aware output encoding

3. Use parameterized/structured rendering APIs

4. Sanitize intentionally permitted HTML

5. Deploy appropriate CSP

6. Protect session cookies

7. Test all rendering contexts

8. Retest after remediation
```

---

## 35. Key Comparison

| Concept | Meaning |
|---|---|
| Reflected XSS | Input returned in a response and executed |
| Stored XSS | Stored input later executed in a browser |
| DOM XSS | Client-side data flow reaches unsafe execution context |
| HTML Injection | HTML structure affected, not necessarily script execution |
| CSP | Browser-enforced content restrictions |
| Output Encoding | Keeps data from becoming executable syntax |
| Sanitization | Removes/restricts unsafe HTML constructs |

---

## Key Takeaway

XSS fundamentally involves:

```text
UNTRUSTED INPUT
       ↓
UNSAFE OUTPUT / DOM HANDLING
       ↓
BROWSER INTERPRETATION
       ↓
SCRIPT EXECUTION
```

The most important defensive principle:

```text
DATA
  !=
EXECUTABLE CODE
```

Use:

```text
CONTEXT-AWARE ENCODING
        +
SAFE DOM APIs
        +
HTML SANITIZATION
        +
CSP
```

to maintain that boundary.

**XSS testing should identify the exact source, execution context, vulnerable sink, and security impact—not merely whether a generic payload produces an alert.**
