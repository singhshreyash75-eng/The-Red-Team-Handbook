# 🌐 HTTP (HyperText Transfer Protocol)

> HTTP is the foundation of web communication. Every website, API, web application, and browser interaction relies on HTTP. As a Red Teamer, understanding HTTP is essential because almost every web vulnerability originates from how HTTP requests and responses are handled.

---

# Overview

HTTP (HyperText Transfer Protocol) is an **application layer protocol** used for communication between clients (browsers) and servers.

Whenever you:

- Open a website
- Submit a login form
- Upload a file
- Use Burp Suite
- Test an API

...you are interacting with HTTP.

---

# Learning Objectives

After this chapter you should understand:

- HTTP Architecture
- Client-Server Model
- Request & Response Structure
- HTTP Methods
- Status Codes
- Headers
- Cookies
- Sessions
- Authentication
- Caching
- HTTP/2 & HTTP/3
- Red Team Perspective

---

# Client-Server Architecture

```text
Browser (Client)
        │
HTTP Request
        │
        ▼
Web Server
        │
HTTP Response
        │
        ▼
Browser
```

The browser sends a **request**.

The server processes it and sends back a **response**.

---

# HTTP Request Structure

Example:

```http
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: */*

```

Components:

- Request Line
- Headers
- Body (optional)

---

# HTTP Response Structure

```http
HTTP/1.1 200 OK
Server: nginx
Content-Type: text/html

<html>...</html>
```

Components:

- Status Line
- Headers
- Response Body

---

# HTTP Methods

| Method | Purpose |
|---------|----------|
| GET | Retrieve data |
| POST | Send data |
| PUT | Replace resource |
| PATCH | Update resource |
| DELETE | Remove resource |
| OPTIONS | Show allowed methods |
| HEAD | Response headers only |

---

## GET

```http
GET /profile HTTP/1.1
```

Used for:

- Viewing pages
- Fetching data

Should **not** modify server data.

---

## POST

```http
POST /login HTTP/1.1
```

Used for:

- Login
- Registration
- File Upload
- Forms

Usually contains a request body.

---

## PUT

Replace an existing resource.

---

## PATCH

Modify only part of a resource.

---

## DELETE

Deletes resources.

Example:

```http
DELETE /users/15
```

---

# HTTP Headers

Headers provide metadata.

Common Request Headers

| Header | Purpose |
|---------|----------|
| Host | Target host |
| User-Agent | Browser information |
| Accept | Accepted content types |
| Cookie | Session cookies |
| Authorization | Credentials |
| Referer | Previous page |
| Origin | Request origin |

Common Response Headers

| Header | Purpose |
|---------|----------|
| Server | Web Server |
| Set-Cookie | Create Cookie |
| Content-Type | MIME Type |
| Cache-Control | Caching |
| Location | Redirect |
| Content-Length | Body Size |

---

# Cookies

Cookies store information on the client.

Example

```http
Set-Cookie:
PHPSESSID=abc123
```

Common uses:

- Login sessions
- Preferences
- Shopping carts

---

# Sessions

A session stores user state.

Typical flow:

```text
Login

↓

Server creates Session

↓

Session ID stored in Cookie

↓

Browser sends Cookie

↓

Authenticated
```

---

# URL Components

Example

```
https://example.com/login?id=5
```

Breakdown

```
Protocol

↓

Host

↓

Path

↓

Parameter
```

---

# HTTP Status Codes

## 1xx

Informational

---

## 2xx

Success

| Code | Meaning |
|------:|---------|
|200|OK|
|201|Created|
|204|No Content|

---

## 3xx

Redirection

| Code | Meaning |
|------:|---------|
|301|Moved Permanently|
|302|Temporary Redirect|
|304|Not Modified|

---

## 4xx

Client Errors

| Code | Meaning |
|------:|---------|
|400|Bad Request|
|401|Unauthorized|
|403|Forbidden|
|404|Not Found|
|405|Method Not Allowed|
|429|Too Many Requests|

---

## 5xx

Server Errors

| Code | Meaning |
|------:|---------|
|500|Internal Server Error|
|502|Bad Gateway|
|503|Service Unavailable|

---

# Authentication

Common mechanisms:

- Basic Auth
- Bearer Token
- JWT
- OAuth
- API Keys

---

# Caching

Caching reduces server load.

Headers:

```http
Cache-Control

Expires

ETag
```

---

# HTTP Keep-Alive

Instead of opening a new TCP connection for every request, HTTP can reuse existing connections.

This improves performance.

---

# HTTP/2

Features:

- Multiplexing
- Header Compression
- Faster Performance

---

# HTTP/3

Uses:

- QUIC
- UDP instead of TCP

Improves speed and reliability.

---

# Red Team Perspective

When testing applications, pay attention to:

- Methods
- Headers
- Cookies
- Authentication
- Session IDs
- Parameters
- Hidden Endpoints
- Content Types
- File Upload Requests

Burp Suite allows interception and modification of every HTTP request.

Most web vulnerabilities originate from manipulating HTTP requests.

---

# Common Beginner Mistakes

- Confusing HTTP with HTML
- Ignoring headers
- Ignoring cookies
- Not checking different HTTP methods
- Assuming GET requests are always safe

---

# Interview Questions

- What is HTTP?
- Difference between GET and POST?
- What are HTTP headers?
- What are cookies?
- Difference between Cookies and Sessions?
- Explain HTTP Status Codes.
- What is HTTP Keep-Alive?
- Difference between HTTP/1.1, HTTP/2 and HTTP/3?

---

# Quick Revision

- HTTP is an Application Layer protocol.
- Browser sends Request.
- Server sends Response.
- GET retrieves data.
- POST sends data.
- Cookies are stored on the client.
- Sessions are maintained on the server.
- Headers contain metadata.
- Status Codes indicate request results.
- Burp Suite works by intercepting HTTP traffic.

---

# Practice

- PortSwigger Web Security Academy
- TryHackMe — HTTP in Detail
- Hack The Box Academy — Web Requests

---

# References

- RFC 9110
- MDN Web Docs
- PortSwigger Web Security Academy
- OWASP HTTP Cheat Sheet
