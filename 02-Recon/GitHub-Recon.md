# 🐙 GitHub Reconnaissance

> GitHub reconnaissance is the process of reviewing publicly available repositories, commits, code, configuration examples, and organization metadata to understand a target's publicly exposed development footprint.

---

## Overview

Modern organizations use GitHub for:

- Source code
- Documentation
- Infrastructure-as-Code
- CI/CD
- API clients
- Configuration examples
- Open-source projects

Public repositories can therefore reveal useful information about an organization's technology stack and application architecture.

The objective is **not simply to search for passwords**.

Good GitHub recon answers:

> What can public development artifacts tell me about the target?

---

## What Can GitHub Reveal?

Potentially useful information includes:

- Domains
- Subdomains
- API endpoints
- Development environments
- Technology stacks
- Cloud providers
- Internal naming conventions
- API documentation
- Historical configuration
- Employee/usernames
- Public client identifiers
- Accidentally committed secrets

---

## Basic Workflow

```text
Organization
      ↓
Find GitHub Presence
      ↓
Repositories
      ↓
Code Search
      ↓
Commit History
      ↓
Configuration Files
      ↓
Endpoints / Infrastructure
      ↓
Validate In-Scope Findings
```

---

## Finding the Organization

Search GitHub for:

```text
Organization Name
```

and:

```text
example.com
```

Possible results:

```text
Official organization
Employee repositories
Open-source projects
Documentation
Forks
```

Verify that repositories actually relate to the target before drawing conclusions.

---

## Search by Domain

Searching for a known domain can uncover references such as:

```text
api.example.com
dev.example.com
auth.example.com
```

These references may exist in:

- Source files
- Documentation
- Configuration
- Tests
- Commit history

---

## Useful Search Terms

Combine the organization/domain with terms such as:

```text
api
endpoint
url
host
domain
staging
development
production
config
```

For architecture discovery, also look for:

```text
graphql
websocket
oauth
s3
azure
firebase
```

---

## Configuration Files

Pay attention to filenames such as:

```text
.env.example
config.json
settings.json
application.yml
docker-compose.yml
Dockerfile
package.json
requirements.txt
pom.xml
```

These can reveal technologies and expected configuration variables.

Example:

```env
API_URL=https://api.example.com
AUTH_URL=https://auth.example.com
```

Even without credentials, this reveals infrastructure.

---

## Environment Variables

Source code may reference variables such as:

```text
DATABASE_URL
API_URL
AWS_REGION
CLIENT_ID
REDIS_HOST
```

This helps understand application architecture.

A variable name itself is normally not sensitive.

---

## Infrastructure-as-Code

Files such as:

```text
terraform
CloudFormation
Kubernetes YAML
Docker Compose
Ansible
```

may reveal:

- Cloud services
- Internal service names
- Ports
- Network architecture
- Application dependencies

---

## CI/CD Files

Look for:

```text
.github/workflows/
.gitlab-ci.yml
Jenkinsfile
```

They can reveal:

- Build processes
- Deployment environments
- Tooling
- Cloud providers

Secrets referenced through secure secret stores are generally not visible, but their **names** may still explain architecture.

---

## Commit History

Current source code isn't the entire story.

Developers may remove information later while it remains in Git history.

Useful commands for a repository you are authorized to analyze:

```bash
git log --oneline
```

Inspect a commit:

```bash
git show COMMIT_ID
```

Search history:

```bash
git log -S "API_URL"
```

---

## Deleted Files

A configuration file may have existed historically even if it no longer exists in the current branch.

Commit history can therefore reveal:

```text
Old endpoints
Deprecated infrastructure
Previous configuration
```

Treat historical information as potentially stale.

---

## Public Secret Exposure

Occasionally, developers accidentally commit credentials.

Examples can include:

```text
Private keys
Cloud credentials
API tokens
Database credentials
```

Important:

**Finding a credential does not authorize you to use it.**

For bug bounty or pentest work:

1. Preserve minimal evidence.
2. Check program rules.
3. Avoid accessing unrelated resources.
4. Report appropriately.

---

## Secret-Scanning Tools

Tools used for defensive or authorized repository review include:

- Gitleaks
- TruffleHog
- GitHub Secret Scanning

Example against a repository you own or are explicitly authorized to assess:

```bash
gitleaks detect --source .
```

Use current official documentation for exact flags and behavior.

---

## package.json

Example:

```json
{
  "dependencies": {
    "express": "...",
    "jsonwebtoken": "...",
    "axios": "..."
  }
}
```

This reveals:

```text
Node.js
Express
JWT
Axios
```

Useful for technology fingerprinting.

---

## requirements.txt

Example:

```text
Flask
requests
SQLAlchemy
```

Possible stack:

```text
Python
Flask
SQLAlchemy
```

---

## Search for Endpoints

Downloaded authorized source can be searched locally.

Example:

```bash
grep -Rni "https://" .
```

API references:

```bash
grep -Rni "/api/" .
```

GraphQL:

```bash
grep -Rni "graphql" .
```

---

## GitHub Recon + JavaScript Recon

These techniques complement each other.

Example:

```text
Public Repository
      ↓
API Hostname
      ↓
JavaScript Bundle
      ↓
Endpoint
      ↓
In-Scope Application
```

The goal is to connect information.

---

## Employee Repositories

Developers may maintain personal public repositories related to:

- Conference demos
- SDK examples
- Open-source components

Do not broadly profile unrelated personal information.

Focus only on public technical artifacts relevant to the authorized target.

---

## What to Document

Example:

```text
Repository:
example/mobile-app

Technology:
React Native

API:
api.example.com

Authentication:
OAuth

Interesting:
GraphQL endpoint referenced

Source:
Public GitHub repository
```

---

## Red Team Perspective

Poor GitHub recon:

```text
Search "password"
     ↓
Nothing
     ↓
Stop
```

Better GitHub recon:

```text
Repositories
     ↓
Technologies
     ↓
Configuration
     ↓
Endpoints
     ↓
History
     ↓
Infrastructure Relationships
```

Architecture knowledge is often more useful than a random exposed string.

---

## Common Beginner Mistakes

- Searching only for passwords.
- Assuming every repository is official.
- Ignoring commit history.
- Ignoring configuration examples.
- Treating public identifiers as secrets.
- Using discovered credentials without authorization.
- Testing third-party infrastructure.
- Ignoring dependency files.

---

## Interview Questions

### Why is GitHub useful during recon?

It may expose public technical artifacts describing applications and infrastructure.

### Why inspect commit history?

Removed information may remain in earlier commits.

### Is every API key sensitive?

No. Some identifiers are intentionally public; permissions and intended usage determine impact.

### What is secret scanning?

Automated detection of credential-like patterns in repositories and history.

---

## Quick Revision

```text
Organization
   ↓
Repositories
   ↓
Code
   ↓
Config
   ↓
Dependencies
   ↓
Commit History
   ↓
Endpoints / Infrastructure

Look for:
API URLs
Domains
Environment names
Technology stack
IaC
CI/CD
```

---

## References

- GitHub Code Search Documentation
- GitHub Secret Scanning Documentation
- Gitleaks Documentation
- TruffleHog Documentation
- OWASP Web Security Testing Guide
