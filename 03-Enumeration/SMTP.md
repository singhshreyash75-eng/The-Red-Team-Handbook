# ✉️ SMTP Enumeration

> SMTP (Simple Mail Transfer Protocol) is the primary protocol used to transfer email between mail systems. Enumeration focuses on identifying the mail server, supported capabilities, exposed configuration, and—where the server permits it—limited information about mail handling.

---

## Overview

SMTP commonly uses:

```text
25/TCP
```

Mail submission commonly uses:

```text
587/TCP
```

Implicit TLS SMTP is commonly associated with:

```text
465/TCP
```

Do not assume all three ports provide identical functionality.

---

## Mail Flow

Simplified:

```text
Sender
   ↓
Mail Client
   ↓
SMTP Server
   ↓
Recipient Mail Server
   ↓
Recipient
```

SMTP is primarily used to **send/relay mail**.

Protocols such as IMAP and POP3 are commonly used for mailbox retrieval.

---

## Initial Enumeration

```bash
nmap -sV -p 25,465,587 TARGET
```

For discovered SMTP services:

```bash
nmap -sC -sV -p 25,465,587 TARGET
```

Potential information:

```text
Mail product
Version clues
Hostname
TLS support
Capabilities
```

---

## Banner Grabbing

For plaintext SMTP:

```bash
nc TARGET 25
```

Possible response:

```text
220 mail.example.test ESMTP
```

This may reveal:

```text
Hostname
Mail software
Environment information
```

---

## SMTP Commands

SMTP is a text-based protocol.

Common commands include:

```text
HELO
EHLO
MAIL FROM
RCPT TO
DATA
QUIT
```

---

## EHLO

Modern SMTP clients commonly begin with:

```text
EHLO client.example
```

The server may respond with supported capabilities.

Example:

```text
250-STARTTLS
250-SIZE
250-AUTH ...
```

This is useful during enumeration.

---

## HELO vs EHLO

`HELO` belongs to the original SMTP model.

`EHLO` is used by Extended SMTP (ESMTP) and allows the server to advertise extensions.

Prefer understanding the server's `EHLO` response when assessing capabilities.

---

## STARTTLS

Some SMTP servers begin with a plaintext connection and then upgrade it to TLS using:

```text
STARTTLS
```

You can inspect TLS-enabled SMTP using OpenSSL in an authorized environment.

Example:

```bash
openssl s_client -starttls smtp -connect TARGET:25
```

For submission:

```bash
openssl s_client -starttls smtp -connect TARGET:587
```

---

## SMTP Authentication

Mail submission services may support authentication mechanisms.

The server's `EHLO` response may advertise:

```text
AUTH
```

Do not attempt credential attacks unless explicitly permitted.

During enumeration, document:

```text
Authentication advertised?
TLS required?
Which port?
```

---

## User Enumeration

Historically, some SMTP servers exposed commands such as:

```text
VRFY
EXPN
```

that could reveal information about users or mailing lists.

Modern production servers frequently disable or restrict them.

Example in a controlled lab:

```text
VRFY alice
```

Possible responses vary by server.

Do not assume these commands will work or that differing responses definitively prove an account exists.

---

## Nmap SMTP Scripts

List available scripts:

```bash
ls /usr/share/nmap/scripts/smtp*
```

Possible script purposes include:

- Capability enumeration
- Open-relay checks
- User-enumeration checks

Read script documentation before execution because some checks generate actual SMTP transactions.

---

## Open Relay Concept

An **open relay** is a mail server that improperly allows unauthorized users to relay mail to external destinations.

Conceptually:

```text
Unauthorized Sender
       ↓
Target SMTP Server
       ↓
External Recipient
```

Modern mail servers should restrict relaying.

Testing relay behavior can result in real email delivery, so it should only be performed when explicitly authorized and using controlled recipient addresses.

---

## DNS + SMTP

SMTP enumeration should be combined with DNS.

Check MX records:

```bash
dig MX example.com
```

Possible result:

```text
mail.example.com
```

Then inspect the authorized mail infrastructure.

---

## SPF

TXT records may contain SPF policy.

Example:

```bash
dig TXT example.com
```

SPF helps specify which servers are authorized to send mail for a domain.

---

## DKIM

DomainKeys Identified Mail uses cryptographic signatures to help authenticate email.

DKIM public keys are published in DNS under selector-specific records.

---

## DMARC

DMARC provides policy and reporting around SPF/DKIM alignment.

A DMARC record typically exists under:

```text
_dmarc.example.com
```

Query:

```bash
dig TXT _dmarc.example.com
```

These technologies become more important in phishing-defense and Blue Team material.

---

## Enumeration Checklist

When SMTP is exposed:

```text
□ Port
□ Banner
□ Hostname
□ Product/version clues
□ EHLO capabilities
□ TLS / STARTTLS
□ Authentication requirements
□ MX relationship
□ SPF
□ DKIM/DMARC context
□ Relay policy, only if authorized
```

---

## Example Notes

```text
Domain:
example.com

MX:
mail.example.com

Ports:
25,587

SMTP:
ESMTP

STARTTLS:
Supported

Authentication:
Advertised on submission service

DMARC:
Present
```

---

## Red Team Perspective

SMTP should not be reduced to:

```text
Port 25
   ↓
Find Users
```

A better approach:

```text
DNS
 ↓
Mail Infrastructure
 ↓
SMTP Capabilities
 ↓
TLS
 ↓
Authentication
 ↓
Email Security Configuration
```

This gives a much more accurate picture of the organization's mail environment.

---

## Common Beginner Mistakes

- Assuming SMTP always runs only on port 25.
- Confusing SMTP with IMAP/POP3.
- Expecting `VRFY` to work on modern servers.
- Testing open relay by sending uncontrolled email.
- Brute-forcing mail accounts without authorization.
- Ignoring TLS.
- Ignoring MX, SPF, DKIM, and DMARC context.

---

## Interview Questions

### What is SMTP?

A protocol primarily used for transferring email.

### Common SMTP-related ports?

25 for server-to-server SMTP, 587 for message submission, and 465 for implicit TLS submission in modern deployments.

### HELO vs EHLO?

EHLO enables Extended SMTP capabilities.

### What is STARTTLS?

A mechanism for upgrading a plaintext SMTP connection to TLS.

### What is an open relay?

A mail server that improperly forwards mail for unauthorized senders.

### SMTP vs IMAP?

SMTP sends/transfers mail; IMAP provides mailbox access/synchronization.

---

## Quick Revision

```text
Ports:
25
465
587

Commands:
HELO
EHLO
MAIL FROM
RCPT TO
DATA
QUIT

Check:
Banner
Capabilities
STARTTLS
Authentication
MX
SPF
DKIM
DMARC

Useful:
dig MX example.com
dig TXT example.com
openssl s_client -starttls smtp -connect TARGET:25
```

---

## Practice

- TryHackMe mail-service labs
- Hack The Box
- HTB Academy
- Your own mail lab

---

## References

- RFC 5321 — SMTP
- RFC 7208 — SPF
- RFC 6376 — DKIM
- RFC 7489 — DMARC
- Nmap NSE Documentation
