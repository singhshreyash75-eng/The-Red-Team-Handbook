# John the Ripper

> Offline password auditing and hash analysis for authorized security assessments and labs.

## What is John the Ripper?

**John the Ripper (JtR)** is a password-security auditing and recovery tool.

It is primarily used for:

```text
Password hash auditing
Offline password recovery
Password-strength testing
CTFs
Forensic analysis
Protected-file auditing
```

Conceptually:

```text
Password Hash
      ↓
John the Ripper
      ↓
Candidate Passwords
      ↓
Verification
      ↓
Match
```

---

# John is Primarily Offline

John generally operates on authentication material already available to the tester.

```text
Hash / Password Verifier
          ↓
Local Machine
          ↓
Candidate Testing
```

This is different from sending password guesses to a live authentication service.

---

# John vs Hydra

```text
John
 |
Offline credential material
 |
OFFLINE
```

versus:

```text
Hydra
 |
Remote authentication service
 |
ONLINE
```

Therefore:

```text
John  = Offline Password Auditing
Hydra = Online Authentication Testing
```

---

# John vs Hashcat

Both perform offline password auditing.

### John

Strong in:

```text
Flexible format handling
Rules
CPU-based workflows
Conversion utilities
Automatic format handling
```

### Hashcat

Strong in:

```text
GPU acceleration
Highly optimized hash modes
Large-scale candidate testing
```

Conceptually:

```text
John
   +
Hashcat
   =
Complementary Password Auditing Tools
```

---

# Hashing Refresher

A password-storage system conceptually performs:

```text
Password
   ↓
Password Hash / KDF
   ↓
Stored Verifier
```

John tests:

```text
Candidate Password
       ↓
Same Verification Algorithm
       ↓
Candidate Result
       ↓
Compare
```

Therefore John does not normally:

```text
Decrypt a hash
```

It performs:

```text
Candidate generation
+
Verification
```

---

# Basic Usage

For supported hash material in an authorized lab:

```bash
john hashes.txt
```

John attempts to process the supplied data using supported formats.

Check help:

```bash
john --help
```

Available formats:

```bash
john --list=formats
```

Exact capabilities depend on the installed build.

---

# Wordlist Mode

A common password-audit strategy:

```bash
john --wordlist=wordlist.txt hashes.txt
```

Conceptually:

```text
Wordlist
   |
   +-- password
   +-- welcome
   +-- summer2026
   +-- example123
   |
   v
John
   |
Candidate Verification
```

---

# Rules

John supports rules that transform wordlist entries.

Example base word:

```text
company
```

Possible transformations:

```text
Company
company1
Company123
Company!
Company2026
```

Conceptually:

```text
Wordlist
   ↓
Rules
   ↓
Expanded Candidates
```

Rules are useful because human passwords often follow predictable mutation patterns.

---

# Incremental Mode

John can generate candidate passwords instead of relying only on a wordlist.

Conceptually:

```text
Character Space
      ↓
Candidate Generation
      ↓
Verification
```

Exhaustive searches become extremely expensive as password length increases.

---

# Password Search Space

If:

```text
N = number of possible characters
L = password length
```

the theoretical search space grows approximately as:

```text
N^L
```

Therefore longer passwords dramatically increase brute-force cost.

---

# Salts

A **salt** is unique/random data incorporated into password hashing.

```text
Password
   +
Salt
   ↓
Password Hash
```

Without unique salts:

```text
Same Password
     ↓
Same Hash
```

With salts:

```text
Password + Salt A → Hash A

Password + Salt B → Hash B
```

This reduces the usefulness of precomputed tables.

---

# Password Hashing Algorithms

Modern password storage should use purpose-built password KDFs such as:

```text
Argon2
scrypt
bcrypt
PBKDF2
```

These deliberately make each password guess expensive.

---

# Conversion Utilities

One major strength of John is its collection of conversion utilities.

Conceptually:

```text
Protected File
      ↓
*2john Utility
      ↓
John-Compatible Data
      ↓
John
```

Utilities commonly follow naming patterns such as:

```text
something2john
```

---

# Archive Auditing

For an authorized password-protected archive:

```text
ZIP / Archive
      ↓
Extract Password Verifier
      ↓
John-Compatible Data
      ↓
John
      ↓
Password Audit
```

The conversion utility does not normally recover the plaintext password itself.

It extracts material that John can test offline.

---

# Document Auditing

Similar workflows can exist for supported:

```text
PDF
Office documents
Archives
Encrypted containers
```

Conceptually:

```text
Protected Document
       ↓
Extract Verifier
       ↓
John
       ↓
Candidate Testing
```

---

# Linux Password Hashes

Linux commonly stores account information across:

```text
/etc/passwd
/etc/shadow
```

`/etc/shadow` contains sensitive password-verification information and is normally protected.

In authorized labs, John provides workflows for preparing supported Unix credential material for auditing.

---

# NT Hashes

Windows environments may contain:

```text
NT hashes
```

Conceptually:

```text
Windows Password
       ↓
NT Hash
       ↓
Offline Password Audit
```

NT hashes are comparatively fast to compute, so password strength becomes especially important.

---

# Show Recovered Passwords

A commonly encountered command:

```bash
john --show hashes.txt
```

This displays previously recovered results associated with the supplied input.

Recovered passwords are sensitive.

---

# Sessions

Long-running jobs may use session/recovery functionality.

Conceptually:

```text
Start
 ↓
Save State
 ↓
Stop
 ↓
Resume
```

This avoids restarting expensive password-auditing jobs.

---

# Password Audit Strategy

A sensible order:

```text
Hash Identification
        ↓
Common Passwords
        ↓
Targeted Wordlist
        ↓
Rules
        ↓
Hybrid / Pattern-Based Candidates
        ↓
Broader Search if Justified
```

Do not immediately start enormous brute-force searches.

---

# Weak Password Patterns

Common human patterns include:

```text
Company name
Season
Year
Name
Keyboard sequence
Simple suffix
```

Examples:

```text
Company2026!
Summer2026
Welcome123
```

Targeted auditing tests whether the password policy permits these weak patterns.

---

# Credential Handling

Recovered passwords must be treated as sensitive assessment data.

Do not:

```text
Commit to Git
Reuse outside scope
Publish in screenshots
Share unnecessarily
Store indefinitely
```

Follow the engagement's data-retention requirements.

---

# Defensive Lessons

Protect password databases using:

```text
Strong unique passwords
Password managers
Modern password KDFs
Unique salts
MFA
Credential database protection
```

---

# Quick Comparison

```text
John the Ripper
      |
      +--> Offline
      +--> Hash auditing
      +--> Rules
      +--> Format conversion

Hashcat
      |
      +--> Offline
      +--> GPU acceleration

Hydra
      |
      +--> Online
      +--> Remote authentication
```

---

# Key Takeaway

John's mental model:

```text
PASSWORD VERIFIER
       +
CANDIDATES
       ↓
OFFLINE VERIFICATION
       ↓
MATCH
```

It does not magically reverse secure hashes.

The security lesson is:

```text
If password-verification material
is stolen, the strength of the password
and the password-storage algorithm
determine resistance to offline guessing.
```
