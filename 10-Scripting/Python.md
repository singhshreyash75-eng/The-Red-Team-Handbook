# Python

> Python scripting fundamentals for security automation, networking, web interaction, data processing, and tool development.

## What is Python?

**Python** is a high-level, interpreted, general-purpose programming language.

It is heavily used in cybersecurity for:

```text
Automation
Reconnaissance
Networking
Web/API interaction
Data processing
Log analysis
Security tooling
Exploit research
File parsing
```

Many security tools and PoCs are written in Python.

---

# Why Python for Security?

Python provides:

```text
Simple syntax
Large standard library
Huge package ecosystem
Cross-platform support
Fast development
Strong networking support
Easy automation
```

Conceptually:

```text
Security Task
     ↓
Python Script
     ↓
Automated Workflow
```

---

# Check Python

```bash
python3 --version
```

Interactive interpreter:

```bash
python3
```

Exit:

```python
exit()
```

---

# First Script

Create:

```text
hello.py
```

Code:

```python
print("Hello, security!")
```

Run:

```bash
python3 hello.py
```

---

# Comments

Single-line comment:

```python
# This is a comment
```

Comments should explain:

```text
Why
```

rather than simply repeat what obvious code does.

---

# Variables

Python does not require explicit type declarations for normal variables.

```python
name = "Alice"
port = 443
alive = True
```

Print:

```python
print(name)
print(port)
```

---

# Basic Data Types

Important types:

```text
str
int
float
bool
list
tuple
dict
set
bytes
None
```

Example:

```python
domain = "example.com"
port = 443
timeout = 5.0
alive = True
```

---

# Strings

```python
domain = "example.com"
```

Useful operations:

```python
print(domain.upper())
print(domain.lower())
print(len(domain))
```

---

# f-Strings

Modern Python provides convenient string formatting:

```python
domain = "example.com"

print(f"Target: {domain}")
```

Output:

```text
Target: example.com
```

---

# User Input

```python
domain = input("Enter domain: ")

print(f"Target: {domain}")
```

Never directly treat arbitrary input as trusted.

---

# Lists

Lists store ordered collections.

```python
ports = [80, 443, 8080]
```

Access:

```python
print(ports[0])
```

Output:

```text
80
```

---

# Append

```python
ports.append(8443)
```

Now:

```text
80
443
8080
8443
```

---

# Tuples

Tuples are ordered and normally treated as immutable.

```python
target = ("example.com", 443)
```

Useful for fixed groups of values.

---

# Dictionaries

Dictionaries store key/value pairs.

```python
services = {
    80: "HTTP",
    443: "HTTPS"
}
```

Access:

```python
print(services[443])
```

Output:

```text
HTTPS
```

---

# Sets

Sets store unique values.

```python
hosts = {
    "api.example.com",
    "www.example.com"
}
```

Useful for deduplication.

Example:

```python
hosts = list(set(hosts))
```

---

# Conditions

```python
port = 443

if port == 443:
    print("HTTPS")
else:
    print("Other")
```

Python uses indentation to define code blocks.

---

# Comparison Operators

```text
==    equal
!=    not equal
>     greater
<     less
>=    greater/equal
<=    less/equal
```

Logical operators:

```text
and
or
not
```

---

# For Loop

```python
ports = [80, 443, 8080]

for port in ports:
    print(port)
```

---

# range()

```python
for i in range(5):
    print(i)
```

Output:

```text
0
1
2
3
4
```

---

# While Loop

```python
count = 1

while count <= 5:
    print(count)
    count += 1
```

---

# Functions

Functions provide reusable logic.

```python
def greet(name):
    print(f"Hello {name}")
```

Call:

```python
greet("Alice")
```

---

# Return Values

```python
def add(a, b):
    return a + b
```

Use:

```python
result = add(2, 3)

print(result)
```

---

# Type Hints

Python supports optional type hints:

```python
def check_port(host: str, port: int) -> bool:
    return True
```

Type hints improve:

```text
Readability
Editor support
Maintainability
Static analysis
```

but Python does not normally enforce them automatically at runtime.

---

# Modules

Python functionality can be imported from modules.

Example:

```python
import os
import sys
import socket
```

Specific import:

```python
from pathlib import Path
```

---

# Standard Library

Security-relevant standard modules include:

```text
os
sys
socket
subprocess
json
re
hashlib
base64
urllib
ipaddress
pathlib
argparse
logging
concurrent.futures
```

---

# File Reading

```python
with open("targets.txt", "r") as file:
    data = file.read()

print(data)
```

Using:

```python
with
```

automatically handles closing the file.

---

# Read Line-by-Line

```python
with open("targets.txt", "r") as file:
    for line in file:
        target = line.strip()
        print(target)
```

This is useful for target lists.

---

# Write Files

```python
with open("output.txt", "w") as file:
    file.write("example.com\n")
```

Append:

```python
with open("output.txt", "a") as file:
    file.write("api.example.com\n")
```

---

# pathlib

Modern Python often uses:

```python
pathlib
```

Example:

```python
from pathlib import Path

path = Path("targets.txt")

if path.exists():
    print("File exists")
```

---

# Exceptions

Errors can be handled with:

```text
try
except
finally
```

Example:

```python
try:
    with open("targets.txt") as file:
        print(file.read())
except FileNotFoundError:
    print("File not found")
```

---

# Avoid Broad Exceptions

Avoid unnecessary:

```python
except:
    pass
```

because it hides errors.

Prefer:

```python
except FileNotFoundError:
```

or another specific exception.

---

# Command-Line Arguments

Simple access:

```python
import sys

print(sys.argv)
```

If run as:

```bash
python3 script.py example.com
```

then:

```python
sys.argv[1]
```

contains:

```text
example.com
```

---

# argparse

For proper CLI tools, use:

```text
argparse
```

Example:

```python
import argparse

parser = argparse.ArgumentParser()

parser.add_argument("domain")

args = parser.parse_args()

print(args.domain)
```

Run:

```bash
python3 script.py example.com
```

---

# Environment Variables

Read:

```python
import os

api_key = os.getenv("API_KEY")
```

Environment variables are frequently used for configuration and secrets.

Do not print sensitive values unnecessarily.

---

# JSON

JSON is extremely important for:

```text
APIs
Scanner output
Configuration
Cloud services
Automation
```

Parse:

```python
import json

data = '{"name": "Alice"}'

obj = json.loads(data)

print(obj["name"])
```

---

# JSON File

```python
import json

with open("data.json") as file:
    data = json.load(file)

print(data)
```

Write:

```python
with open("output.json", "w") as file:
    json.dump(data, file, indent=4)
```

---

# Regular Expressions

Python provides:

```python
re
```

Example:

```python
import re

text = "Server: nginx"

match = re.search(r"nginx", text)

if match:
    print("Found")
```

Regex is useful for:

```text
Log parsing
Data extraction
Pattern matching
Recon output processing
```

---

# Base64

Python provides:

```python
base64
```

Example:

```python
import base64

encoded = base64.b64encode(b"hello")

print(encoded)
```

Decode:

```python
decoded = base64.b64decode(encoded)

print(decoded)
```

Remember:

```text
Base64 != Encryption
```

---

# Hashing

Python provides:

```python
hashlib
```

Example:

```python
import hashlib

data = b"hello"

digest = hashlib.sha256(data).hexdigest()

print(digest)
```

Useful for:

```text
File integrity
Checksums
Data identification
```

General-purpose fast hashes should not be used directly for password storage.

---

# Networking with socket

Python's:

```text
socket
```

module provides low-level networking.

Example authorized connectivity check:

```python
import socket

host = "example.com"
port = 443

sock = socket.socket()
sock.settimeout(3)

try:
    sock.connect((host, port))
    print("Connected")
except OSError:
    print("Connection failed")
finally:
    sock.close()
```

---

# Why Timeouts Matter

Without a timeout:

```text
Network Request
      ↓
Remote host never responds
      ↓
Script waits too long
```

Use:

```python
sock.settimeout(...)
```

or equivalent timeout settings in network libraries.

---

# HTTP Requests

A popular third-party Python library is:

```text
requests
```

Example:

```python
import requests

response = requests.get(
    "https://example.com",
    timeout=5
)

print(response.status_code)
```

---

# HTTP Response

Useful properties:

```python
response.status_code
response.headers
response.text
response.content
```

Conceptually:

```text
Python
 ↓
HTTP Request
 ↓
Server
 ↓
HTTP Response
 ↓
Python Analysis
```

---

# Query Parameters

```python
import requests

params = {
    "q": "test"
}

response = requests.get(
    "https://example.com/search",
    params=params,
    timeout=5
)
```

Using structured parameters is preferable to manually concatenating arbitrary input into URLs.

---

# POST Requests

Example:

```python
import requests

data = {
    "name": "Alice"
}

response = requests.post(
    "https://example.com/api/profile",
    json=data,
    timeout=5
)
```

Use only against authorized APIs.

---

# HTTP Headers

```python
headers = {
    "User-Agent": "Security-Lab-Client"
}

response = requests.get(
    "https://example.com",
    headers=headers,
    timeout=5
)
```

---

# Sessions

`requests.Session()` can preserve:

```text
Cookies
Connection settings
Headers
```

across multiple requests.

Conceptually:

```text
Login
 ↓
Session Cookie
 ↓
Further Requests
```

Treat authentication cookies/tokens as credentials.

---

# subprocess

Python can execute external programs using:

```text
subprocess
```

Prefer structured argument lists:

```python
import subprocess

result = subprocess.run(
    ["whoami"],
    capture_output=True,
    text=True
)

print(result.stdout)
```

---

# Avoid shell=True

Avoid constructing commands like:

```python
subprocess.run(
    f"tool {user_input}",
    shell=True
)
```

with untrusted input.

This can create:

```text
Command Injection
```

Prefer:

```python
subprocess.run(
    ["tool", user_input]
)
```

where appropriate.

---

# Command Injection Concept

Dangerous flow:

```text
User Input
    ↓
Shell Command String
    ↓
Shell Parser
    ↓
Unexpected Commands
```

Safer flow:

```text
User Input
    ↓
Argument
    ↓
Direct Process Execution
```

---

# Concurrency

Recon/security tools frequently need to process many authorized targets.

Python provides:

```text
threading
asyncio
concurrent.futures
multiprocessing
```

The correct model depends on the workload.

---

# ThreadPoolExecutor

For network-bound tasks:

```python
from concurrent.futures import ThreadPoolExecutor
```

Conceptually:

```text
Targets
  |
  +--> Worker 1
  +--> Worker 2
  +--> Worker 3
```

This can improve performance.

---

# Concurrency Limits

Do not create unlimited workers.

More concurrency means:

```text
More requests/sec
More network load
More target load
More rate limiting
```

Always respect:

```text
Scope
Rate limits
Rules of engagement
```

---

# asyncio

`asyncio` provides asynchronous I/O.

Conceptually:

```text
One Event Loop
      ↓
Many Waiting Network Tasks
```

This is useful for high-concurrency network applications.

---

# IP Address Handling

Python provides:

```text
ipaddress
```

Example:

```python
import ipaddress

network = ipaddress.ip_network("192.0.2.0/30")

for ip in network.hosts():
    print(ip)
```

Useful for legitimate network automation.

---

# URL Parsing

Use:

```python
urllib.parse
```

rather than manually splitting URLs.

Example:

```python
from urllib.parse import urlparse

url = urlparse("https://example.com:8443/api")

print(url.scheme)
print(url.hostname)
print(url.port)
print(url.path)
```

---

# Logging

Professional scripts should use:

```python
import logging
```

instead of relying entirely on `print()`.

Benefits:

```text
Severity levels
Structured output
File logging
Debugging
Timestamping
```

---

# Secrets

Never hardcode:

```python
API_KEY = "secret-value"
PASSWORD = "password123"
```

inside committed source code.

Prefer:

```text
Environment variables
Secret managers
Configuration excluded from Git
Workload identities
```

---

# Virtual Environments

Python projects should often use:

```text
Virtual Environments
```

Create:

```bash
python3 -m venv .venv
```

Activate on Linux/macOS:

```bash
source .venv/bin/activate
```

This isolates project dependencies.

---

# pip

Python packages are commonly installed with:

```bash
python3 -m pip install <package>
```

Prefer reviewed/pinned dependencies for reproducible security tooling.

---

# requirements.txt

Dependencies can be recorded:

```text
requests==<approved-version>
```

Then installed:

```bash
python3 -m pip install -r requirements.txt
```

Pinned/reviewed versions improve reproducibility.

---

# Dependency Security

Third-party packages are executable code.

Risks include:

```text
Malicious packages
Typosquatting
Compromised dependencies
Vulnerable libraries
Supply-chain attacks
```

Only install packages from trusted sources.

---

# Simple URL Status Checker

Example for authorized targets:

```python
import requests

targets = [
    "https://example.com",
    "https://example.org",
]

for target in targets:
    try:
        response = requests.get(
            target,
            timeout=5
        )

        print(
            target,
            response.status_code
        )

    except requests.RequestException as error:
        print(
            target,
            "ERROR",
            error
        )
```

Conceptually:

```text
Target List
    ↓
HTTP Requests
    ↓
Status Codes
    ↓
Results
```

---

# Recon Automation

Python can orchestrate workflows such as:

```text
Read Domains
     ↓
Normalize
     ↓
Call APIs
     ↓
Parse Results
     ↓
Deduplicate
     ↓
Write JSON/CSV
```

It becomes especially useful when Bash pipelines become too complex.

---

# Python vs Bash

Use Bash when:

```text
Task mainly chains CLI commands
```

Use Python when:

```text
Complex logic
Structured data
APIs
Error handling
Reusable tooling
Concurrency
```

---

# Python vs Go

```text
Python
 |
Fast development
Large ecosystem
Easy scripting
```

versus:

```text
Go
 |
Compiled
High performance
Strong concurrency
Easy binary distribution
```

A common development pattern is:

```text
Prototype in Python
      ↓
Optimize/rebuild if required
```

although many production tools remain entirely in Python.

---

# Security Coding Principles

When writing security automation:

```text
Validate input
Use timeouts
Limit concurrency
Handle exceptions
Avoid shell injection
Protect secrets
Use TLS correctly
Log safely
Respect scope
Pin dependencies where appropriate
```

---

# Key Takeaway

Python security scripting combines:

```text
VARIABLES
   +
DATA STRUCTURES
   +
FUNCTIONS
   +
FILES
   +
HTTP
   +
NETWORKING
   +
APIs
   +
AUTOMATION
```

For practical security work, prioritize:

```text
requests
json
re
socket
subprocess
argparse
pathlib
logging
concurrent.futures
```

The main reason to move from Bash to Python is usually:

```text
The workflow needs more
LOGIC + STRUCTURE + CONTROL
than a shell pipeline can comfortably provide.
```
