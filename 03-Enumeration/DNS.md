# 🌐 DNS Enumeration

> DNS enumeration is the process of querying Domain Name System infrastructure to understand domains, hostnames, mail services, name servers, address mappings, and other DNS information relevant to an authorized target.

---

## Overview

You already covered **DNS fundamentals** in:

```text
00-Fundamentals/DNS.md
```

This chapter focuses specifically on **enumeration**.

The objective is to turn:

```text
example.com
```

into useful infrastructure information such as:

```text
www.example.com
mail.example.com
vpn.example.com
ns1.example.com
```

---

## Common Ports

DNS primarily uses:

```text
53/UDP
```

but also uses:

```text
53/TCP
```

TCP is important for operations such as:

- Zone transfers
- Large DNS responses
- Some DNSSEC-related traffic
- Fallback when UDP responses are truncated

---

## Initial Nmap Scan

```bash
nmap -sV -p 53 TARGET
```

Include UDP where authorized:

```bash
sudo nmap -sU -sV -p 53 TARGET
```

Remember that checking only TCP 53 can miss a DNS service that primarily responds over UDP.

---

## Core DNS Record Types

| Record | Purpose |
|---|---|
| A | IPv4 address |
| AAAA | IPv6 address |
| NS | Authoritative name server |
| MX | Mail exchanger |
| CNAME | Alias |
| TXT | Text/policy/verification data |
| SOA | Zone authority information |
| PTR | Reverse DNS |
| SRV | Service location |

---

## `dig`

`dig` is one of the most useful DNS tools.

Basic:

```bash
dig example.com
```

Short result:

```bash
dig +short example.com
```

---

## A Records

```bash
dig A example.com
```

Possible output:

```text
192.0.2.10
```

---

## AAAA Records

```bash
dig AAAA example.com
```

Useful for discovering IPv6 infrastructure.

Do not ignore IPv6 simply because your initial assessment focuses on IPv4.

---

## Name Servers

```bash
dig NS example.com
```

Potential output:

```text
ns1.provider.example
ns2.provider.example
```

Name servers can reveal:

- DNS providers
- Infrastructure relationships
- Authoritative servers

---

## MX Records

```bash
dig MX example.com
```

Possible result:

```text
mail.example.com
```

MX records help identify email infrastructure.

---

## TXT Records

```bash
dig TXT example.com
```

Potential information includes:

```text
SPF
Domain verification
Third-party services
```

Not every TXT record is security-sensitive.

---

## SOA Record

```bash
dig SOA example.com
```

The Start of Authority record provides zone-related metadata.

It typically includes:

- Primary authoritative server
- Administrative contact representation
- Serial number
- Refresh/retry timers

---

## CNAME Records

```bash
dig CNAME app.example.com
```

CNAMEs may reveal dependencies on:

```text
CDNs
Cloud platforms
SaaS services
Hosting providers
```

They are particularly useful when investigating ownership and architecture.

---

## Reverse DNS

Reverse DNS maps an IP address to a hostname using PTR records.

Example:

```bash
dig -x 192.0.2.10
```

or:

```bash
host 192.0.2.10
```

Reverse DNS can provide naming clues, although PTR records are not always configured.

---

## `host`

Simple lookup:

```bash
host example.com
```

MX:

```bash
host -t MX example.com
```

NS:

```bash
host -t NS example.com
```

---

## `nslookup`

Interactive or direct lookup:

```bash
nslookup example.com
```

Specify DNS server:

```bash
nslookup example.com 8.8.8.8
```

For modern Linux workflows, `dig` is often more flexible, but `nslookup` remains widely available.

---

## Zone Transfers

DNS servers may transfer zone data between authoritative servers.

The operation is known as:

```text
AXFR
```

A misconfigured authoritative server might expose zone contents to unauthorized clients.

Authorized/lab test:

```bash
dig AXFR example.com @ns1.example.com
```

A successful transfer may reveal hostnames and records within that DNS zone.

Modern properly configured DNS servers normally restrict AXFR.

---

## Zone Transfer Workflow

```text
Domain
  ↓
Find NS Records
  ↓
Identify Authoritative Servers
  ↓
Authorized AXFR Check
  ↓
If Allowed
  ↓
Review Zone Records
```

Example:

```bash
dig NS example.com
```

Then:

```bash
dig AXFR example.com @AUTHORIZED_NAMESERVER
```

---

## Why AXFR Matters

If exposed, a zone transfer can potentially provide:

```text
Hostnames
Mail servers
Internal naming clues
Service records
```

A successful AXFR is primarily an information-disclosure/configuration issue.

---

## DNSSEC

DNSSEC provides authenticity and integrity protections for DNS data using cryptographic signatures.

It does **not** encrypt DNS queries.

Relevant records include:

```text
DNSKEY
DS
RRSIG
```

---

## SRV Records

SRV records identify services.

Format conceptually:

```text
_service._protocol.domain
```

They are especially important in environments such as Active Directory.

Later, AD enumeration will use records related to services including:

```text
LDAP
Kerberos
```

---

## DNS + Active Directory

Active Directory relies heavily on DNS.

Conceptually:

```text
Domain
   ↓
DNS
   ↓
Domain Controllers
   ↓
LDAP / Kerberos
```

This is why DNS enumeration becomes extremely important once we reach the AD module.

---

## DNS Wildcards

Some domains resolve arbitrary hostnames.

Example:

```text
anything.example.com
random123.example.com
```

both resolve.

This can generate false positives during subdomain enumeration.

Test a random hostname:

```bash
dig +short random-name-918273.example.com
```

If it resolves, investigate wildcard behavior.

---

## Split-Horizon DNS

Organizations may return different DNS information depending on where the request originates.

Example:

```text
Internet DNS
    ↓
Public Records

Internal DNS
    ↓
Internal Records
```

This is common in enterprise environments.

---

## DNS Cache

DNS results can be cached at:

```text
Browser
Operating System
Resolver
```

Remember that DNS changes may not become visible everywhere immediately because of TTL and caching.

---

## Nmap DNS Scripts

List available scripts:

```bash
ls /usr/share/nmap/scripts/dns*
```

Scripts may perform:

- DNS service discovery
- Recursion checks
- Selected record queries

Review script behavior before using it.

---

## Recursion

A recursive resolver performs lookups on behalf of clients.

Authoritative servers and recursive resolvers serve different purposes.

Misconfigured publicly accessible recursion can create operational/security issues, including abuse in DNS amplification scenarios.

Do not perform amplification testing against public infrastructure.

---

## Enumeration Workflow

```text
53 Open
   ↓
TCP / UDP
   ↓
A / AAAA
   ↓
NS
   ↓
MX
   ↓
TXT
   ↓
SOA
   ↓
CNAME
   ↓
PTR
   ↓
SRV
   ↓
Authorized AXFR Check
   ↓
Correlate Findings
```

---

## Example Notes

```text
Domain:
example.com

A:
192.0.2.10

NS:
ns1.provider.example
ns2.provider.example

MX:
mail.example.com

TXT:
SPF present

AXFR:
Refused

Interesting:
vpn.example.com discovered through related DNS data
```

---

## Red Team Perspective

DNS enumeration isn't:

```text
dig example.com
      ↓
Done
```

Instead:

```text
Domain
 ↓
Records
 ↓
Infrastructure
 ↓
Services
 ↓
Relationships
```

DNS frequently connects otherwise separate pieces of recon.

Example:

```text
MX
 ↓
mail.example.com
 ↓
SMTP Enumeration
```

or:

```text
SRV
 ↓
Domain Controller
 ↓
LDAP / Kerberos
```

---

## Common Beginner Mistakes

- Querying only A records.
- Ignoring UDP 53.
- Forgetting IPv6.
- Ignoring TXT records.
- Not checking CNAME relationships.
- Assuming every DNS server allows AXFR.
- Ignoring reverse DNS.
- Confusing recursive and authoritative DNS.
- Treating third-party DNS infrastructure as automatically in scope.

---

## Interview Questions

### What is DNS enumeration?

Collecting DNS records and related infrastructure information about a target.

### UDP vs TCP DNS?

Normal queries commonly use UDP; TCP is also used for operations such as zone transfers and responses that require it.

### What is AXFR?

A full DNS zone-transfer mechanism.

### What is an NS record?

Identifies authoritative name servers.

### What is an MX record?

Identifies mail exchangers.

### What is a PTR record?

Provides reverse DNS mapping.

### What are SRV records?

Records describing the location of specific services.

### Why is DNS important in Active Directory?

AD uses DNS extensively to locate domain services such as domain controllers.

---

## Quick Revision

```text
Port:
53 UDP/TCP

Commands:

dig A domain
dig AAAA domain
dig NS domain
dig MX domain
dig TXT domain
dig SOA domain
dig CNAME host
dig -x IP

Zone Transfer:
dig AXFR domain @nameserver

Remember:

A     → IPv4
AAAA  → IPv6
NS    → Name Server
MX    → Mail
TXT   → Text/Policy
CNAME → Alias
PTR   → Reverse DNS
SRV   → Service
SOA   → Zone Metadata
```

---

## Practice

- TryHackMe DNS rooms
- Hack The Box Academy
- HTB machines
- Your own DNS server/lab
- Active Directory lab later

---

## References

- RFC 1034 — DNS Concepts
- RFC 1035 — DNS Implementation
- ISC BIND Documentation
- Nmap NSE Documentation
- Microsoft Active Directory DNS Documentation
