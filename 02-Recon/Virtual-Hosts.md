# 🏠 Virtual Host Discovery

> Virtual hosting allows multiple websites or applications to share the same server or IP address. Virtual-host discovery attempts to identify additional applications associated with an authorized target that may not be obvious from ordinary DNS enumeration.

---

## Overview

Suppose a server has the IP:

```text
192.0.2.10
```

The same server might host:

```text
www.example.com
admin.example.com
dev.example.com
api.example.com
```

All four hostnames may resolve to the same server.

The web server decides which application to return based primarily on the HTTP `Host` header.

---

## Why Virtual Hosts Exist

Organizations commonly host multiple applications behind:

- Apache
- nginx
- IIS
- Reverse proxies
- Load balancers
- Cloud infrastructure

Instead of assigning one IP to every website:

```text
One IP
  │
  ├── www.example.com
  ├── api.example.com
  ├── admin.example.com
  └── dev.example.com
```

This conserves infrastructure and simplifies hosting.

---

## DNS vs Virtual Hosts

These concepts are related but different.

### DNS Enumeration

Attempts to discover hostnames through DNS/public sources.

```text
admin.example.com
        ↓
DNS
        ↓
192.0.2.10
```

### Virtual Host Discovery

Tests how an authorized web server responds to different `Host` values.

```http
GET / HTTP/1.1
Host: admin.example.com
```

An application can sometimes exist as a virtual host even when it is not obvious from your initial DNS discovery.

---

## HTTP Host Header

HTTP/1.1 requests normally contain:

```http
GET / HTTP/1.1
Host: www.example.com
```

Changing:

```http
Host: www.example.com
```

to another valid configured hostname can cause the server to route the request to a different application.

---

## Example Architecture

```text
                    192.0.2.10
                         │
                    Web Server
                         │
          ┌──────────────┼──────────────┐
          │              │              │
     www.example     admin.example    dev.example
          │              │              │
       Website        Admin App       Dev App
```

Same IP.

Different applications.

---

## Manual Testing

Against an authorized lab/server, a request can be sent with a specific Host header:

```bash
curl -H "Host: www.example.com" http://192.0.2.10/
```

Compare with:

```bash
curl -H "Host: admin.example.com" http://192.0.2.10/
```

Different responses may indicate separate virtual hosts.

---

## Establish a Baseline

Before discovery, send a deliberately nonexistent hostname:

```bash
curl -H "Host: random-does-not-exist.example.com" http://192.0.2.10/
```

Record:

- Status code
- Response length
- Page title
- Redirect
- Body

Example:

```text
Status: 200
Length: 4210
Title: Default Site
```

This becomes your baseline.

---

## Why Baselines Matter

Some servers return the same default page for every unknown hostname.

Example:

```text
random1.example.com → 4210 bytes
random2.example.com → 4210 bytes
random3.example.com → 4210 bytes
```

But:

```text
admin.example.com → 8932 bytes
```

The response difference makes `admin` interesting.

---

## FFUF Virtual Host Discovery

In an authorized environment:

```bash
ffuf -w subdomains.txt \
-H "Host: FUZZ.example.com" \
-u http://192.0.2.10/
```

Conceptually:

```text
admin
dev
api
test
```

becomes:

```http
Host: admin.example.com
Host: dev.example.com
Host: api.example.com
Host: test.example.com
```

---

## Filtering Responses

If the default response has a known size, filtering it can reduce noise.

The exact FFUF filtering options should be checked against the installed version's documentation.

The general idea is:

```text
Generate candidates
       ↓
Remove baseline responses
       ↓
Investigate differences
```

---

## Gobuster VHost Mode

Gobuster also supports virtual-host discovery.

Typical authorized/lab usage:

```bash
gobuster vhost \
-u http://example.com \
-w wordlist.txt
```

Check the current Gobuster documentation for version-specific flags.

---

## Burp Suite Workflow

You can test manually using Repeater.

```text
Capture Request
      ↓
Send to Repeater
      ↓
Change Host Header
      ↓
Send
      ↓
Compare Response
```

Compare:

- Status
- Length
- Headers
- Title
- Redirect behavior

---

## Interesting Virtual Host Names

Common naming conventions include:

```text
admin
api
auth
beta
dashboard
dev
internal
legacy
portal
preprod
stage
staging
test
uat
vpn
```

These names are useful prioritization clues, not evidence of vulnerability.

---

## Development Environments

Virtual-host discovery may reveal environments such as:

```text
dev.example.com
staging.example.com
test.example.com
```

These can differ from production through:

- Different versions
- Debug functionality
- Additional endpoints
- Different authentication controls
- Test features

However, **development infrastructure must still be explicitly in scope before deeper testing**.

---

## `/etc/hosts`

In labs, a discovered hostname may not exist in public DNS.

You can locally map an authorized lab hostname:

```text
192.0.2.10 admin.example.test
```

inside:

```text
/etc/hosts
```

Then:

```bash
curl http://admin.example.test
```

Your system sends the appropriate hostname while connecting to the specified IP.

---

## Editing `/etc/hosts`

Linux/macOS:

```bash
sudo nano /etc/hosts
```

Example lab entry:

```text
10.10.10.10 admin.target.htb
```

This technique is very common in Hack The Box environments.

---

## Host Header vs SNI

HTTPS introduces another concept:

```text
SNI
```

Server Name Indication tells the TLS server which hostname the client wants **during the TLS handshake**.

Simplified HTTPS flow:

```text
Client
   ↓
TLS SNI
   ↓
TLS Connection
   ↓
HTTP Host Header
   ↓
Application
```

Because of this, HTTPS virtual-host testing can behave differently from plain HTTP.

---

## Reverse Proxies

Modern applications may sit behind:

```text
nginx
Cloudflare
AWS ALB
Traefik
HAProxy
Kubernetes Ingress
```

Routing may depend on hostname.

Example:

```text
api.example.com
       ↓
Reverse Proxy
       ↓
API Service
```

while:

```text
admin.example.com
       ↓
Reverse Proxy
       ↓
Admin Service
```

---

## Virtual Hosts and Subdomains

A useful workflow is:

```text
Subdomain Enumeration
        ↓
DNS Resolution
        ↓
IP Grouping
        ↓
Virtual Host Discovery
        ↓
Additional Applications
```

This is why virtual-host discovery belongs near the end of Recon.

---

## What to Record

Example:

```text
IP:
192.0.2.10

Baseline:
404 / 2210 bytes

Virtual Host:
admin.example.com

Response:
200 / 9210 bytes

Title:
Administration Portal

Notes:
Authentication required
```

---

## Red Team Perspective

Virtual-host discovery answers:

> Is this IP serving applications that my DNS recon did not clearly reveal?

A strong recon process combines:

```text
DNS
+
HTTP
+
Virtual Hosts
+
Historical Data
+
JavaScript
```

rather than depending on any one technique.

---

## Common Beginner Mistakes

- Confusing subdomain enumeration with vhost discovery.
- Not creating a baseline.
- Treating every response as valid.
- Ignoring response length.
- Forgetting HTTPS/SNI behavior.
- Fuzzing out-of-scope infrastructure.
- Forgetting to add lab hostnames to `/etc/hosts`.
- Assuming an interesting hostname is automatically vulnerable.

---

## Interview Questions

### What is virtual hosting?

Hosting multiple websites or applications on the same server/IP.

### How does HTTP identify the intended virtual host?

Primarily through the `Host` header.

### DNS enumeration vs virtual-host discovery?

DNS enumeration discovers names through DNS/public sources; vhost discovery investigates hostname-based HTTP routing.

### Why establish a baseline?

To distinguish real applications from the server's default response.

### What is SNI?

Server Name Indication communicates the requested hostname during the TLS handshake.

---

## Quick Revision

```text
One IP
  ↓
Multiple Applications

HTTP:
Host: admin.example.com

Workflow:

Get IP
  ↓
Baseline
  ↓
Fuzz Host Header
  ↓
Compare Responses
  ↓
Verify
  ↓
Add authorized lab host to /etc/hosts

Tools:
FFUF
Gobuster
Burp Repeater
curl
```

---

## Practice

Excellent places to practice:

- Hack The Box machines
- HTB Academy
- TryHackMe
- Local nginx/Apache virtual-host labs

---

## References

- RFC 9110 — HTTP Semantics
- RFC 6066 — TLS Extensions / SNI
- FFUF Documentation
- Gobuster Documentation
- OWASP Web Security Testing Guide
