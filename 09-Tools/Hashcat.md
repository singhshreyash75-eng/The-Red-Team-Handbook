# Hashcat

> Hashcat fundamentals for authorized password auditing, hash identification, attack modes, rules, and defensive password analysis.

## What is Hashcat?

**Hashcat** is a high-performance password-recovery and password-auditing tool.

It attempts to recover plaintext candidates by computing hashes and comparing them against supplied password hashes.

Conceptually:

```text
Stored Hash
    |
    v
Hashcat

Candidate Password
    |
Hash Function
    |
    v
Candidate Hash
    |
Compare
    |
    +--> Match -> Password Candidate Found
```

Hashcat is commonly used for authorized:

```text
Password auditing
CTFs
Digital forensics
Security research
Credential-strength assessment
```

---

# Hashing

A cryptographic hash function transforms input into a fixed-format output.

Conceptually:

```text
password
   |
Hash Function
   |
   v
Hash Value
```

For password storage, secure systems use dedicated password-hashing/KDF schemes rather than fast general-purpose hashes.

---

# Hashing != Encryption

Critical distinction:

```text
Encryption
    |
Key
    |
Reversible with correct key
```

versus:

```text
Hashing
    |
One-way transformation
```

Password recovery tools generally:

```text
Guess Candidate
      ↓
Hash Candidate
      ↓
Compare
```

They do not normally "decrypt" a password hash.

---

# Basic Hashcat Model

Hashcat needs:

```text
Hash
+
Correct Hash Mode
+
Candidate Generation Strategy
```

Conceptually:

```text
hashes.txt
    +
Wordlist / Mask / Rules
    ↓
Hashcat
    ↓
Matches
```

---

# Hash Modes

Hashcat uses numeric:

```text
-m
```

values to specify hash algorithms/formats.

Examples vary by format.

Never guess the mode.

Check:

```bash
hashcat --help
```

or Hashcat's official example-hash documentation.

---

# Why Hash Identification Matters

Suppose a hash is:

```text
32 hexadecimal characters
```

That does not uniquely identify the algorithm.

Several formats can look similar.

Therefore:

```text
Hash Appearance
      ↓
Context
      ↓
Candidate Format
      ↓
Verify
```

Context is usually more reliable than length alone.

---

# Example Hash Sources

In authorized environments, hashes might come from:

```text
Lab challenge
Password audit export
Forensic image
Application database provided for assessment
Test Active Directory environment
```

Do not obtain or crack credentials outside the agreed authorization.

---

# Wordlist Attack

A wordlist attack tries candidate passwords from a file.

Conceptually:

```text
password
welcome
summer2026
example123
    |
    v
Hash each candidate
    |
Compare
```

A typical Hashcat command structure is:

```bash
hashcat -m <mode> hashes.txt wordlist.txt
```

Exact options should be verified for the installed version.

---

# Why Wordlists Work

Human passwords often follow predictable patterns:

```text
Names
Words
Years
Seasons
Keyboard patterns
Company-related terms
```

Therefore a relatively small targeted list can sometimes outperform enormous random search spaces.

---

# Dictionary != Password Database

A wordlist is simply:

```text
Candidate passwords
```

Examples may include:

```text
Common passwords
Words
Organization-specific test terms
Generated candidates
```

Use wordlists responsibly because real breached-password datasets can contain sensitive material.

---

# Rules

Rules transform wordlist entries.

Suppose:

```text
password
```

Rules might generate:

```text
Password
password1
password123
Password2026
password!
```

Conceptually:

```text
Base Word
    ↓
Rule
    ↓
Modified Candidates
```

Rules model common human password mutations.

---

# Why Rules Matter

Users rarely choose only dictionary words.

They often transform them:

```text
company
Company
Company1
Company123
Company!
```

Rule-based auditing can test these patterns efficiently.

---

# Mask Attacks

A mask describes a candidate pattern.

Conceptually:

```text
????2026
```

or a structured character pattern.

Masks are useful when password structure is already known from an authorized test scenario.

---

# Character Classes

Hashcat mask syntax provides character classes for categories such as:

```text
Lowercase
Uppercase
Digits
Symbols
```

Check:

```bash
hashcat --help
```

for exact syntax and custom charset options.

---

# Search Space

Suppose a password contains:

```text
8 arbitrary characters
```

The number of possibilities can become enormous.

Conceptually:

```text
Character Set Size ^ Password Length
```

Increasing password length dramatically increases brute-force cost.

---

# Brute Force

Brute force attempts all candidates in a defined search space.

Conceptually:

```text
aaaa
aaab
aaac
...
```

This becomes infeasible rapidly as:

```text
Length
+
Character diversity
```

increase.

---

# Combinator Attack

A combinator-style strategy combines words.

Conceptually:

```text
List A       List B

summer       2026
admin        123
company      !
```

Possible candidates:

```text
summer2026
admin123
company!
```

Useful when known password patterns combine multiple words/components.

---

# Hybrid Attacks

Hybrid approaches combine strategies.

Conceptually:

```text
Wordlist
   +
Mask
```

Example pattern:

```text
Word
+
Four digits
```

This can efficiently model human password behavior.

---

# Salts

A **salt** is unique/random data incorporated into password hashing.

Conceptually:

```text
Password
   +
Salt
   ↓
Password Hash
```

Without salts:

```text
Same Password
     ↓
Same Hash
```

With unique salts:

```text
Same Password + Salt A -> Hash A

Same Password + Salt B -> Hash B
```

This reduces the usefulness of precomputed hash tables and cross-user equality comparisons.

---

# Salt Does Not Make Weak Passwords Strong

A salt protects against certain precomputation attacks.

But:

```text
Weak password
      +
Known salt
```

can still be guessed offline.

Therefore secure storage also needs a deliberately expensive password hashing/KDF scheme.

---

# Password Hashing Algorithms

Modern password storage should use algorithms designed to make guessing expensive.

Examples include:

```text
Argon2
scrypt
bcrypt
PBKDF2
```

These are intentionally more expensive than fast general-purpose hashes.

---

# Fast vs Slow Hashes

Fast hash:

```text
Candidate
   ↓
Very fast computation
   ↓
Many guesses/sec
```

Password hashing/KDF:

```text
Candidate
   ↓
Expensive computation
   ↓
Fewer guesses/sec
```

For password storage:

```text
Slower is intentionally better.
```

---

# GPU Acceleration

Hashcat is known for GPU acceleration.

GPUs can perform many parallel computations.

Conceptually:

```text
CPU
 |
Fewer parallel operations
```

versus:

```text
GPU
 |
Many parallel operations
```

The actual advantage depends heavily on the hash algorithm.

---

# Cracking Speed Is Algorithm Dependent

Do not ask:

```text
How many passwords per second
can this GPU crack?
```

without specifying the hash type.

A fast hash and a memory-hard password KDF can differ by orders of magnitude.

---

# Potfile

Hashcat can remember previously recovered hashes in a:

```text
potfile
```

Conceptually:

```text
Hash
   ↓
Recovered Candidate
   ↓
Potfile
```

This avoids repeating completed work.

Potfiles may contain sensitive credential information and should be protected.

---

# Show Recovered Results

Hashcat provides mechanisms to display already recovered entries.

Check:

```bash
hashcat --help
```

for the current command syntax.

Treat recovered plaintext passwords as highly sensitive assessment data.

---

# Sessions

Long-running audits can use session/checkpoint functionality.

Conceptually:

```text
Start Audit
    ↓
Save State
    ↓
Stop
    ↓
Resume
```

This is useful for controlled long-running password assessments.

---

# Workload and Hardware

Password auditing can consume significant:

```text
GPU
CPU
Power
Cooling
```

Monitor temperatures and hardware health during long runs.

---

# Hashcat + Kerberoasting

In authorized AD labs:

```text
Kerberoasting
      ↓
Password-verification material
      ↓
Offline Password Audit
      ↓
Hashcat
```

The security issue is generally:

```text
Weak service-account password
```

not the mere existence of Kerberos tickets.

---

# Hashcat + AS-REP Roasting

Likewise:

```text
AS-REP material
      ↓
Hashcat
      ↓
Offline password audit
```

The underlying weakness combines:

```text
Preauthentication disabled
       +
Weak password
```

---

# Hashcat vs John the Ripper

Both perform password auditing.

```text
Hashcat
 |
Strong GPU-oriented cracking
and many optimized hash modes
```

```text
John the Ripper
 |
Flexible password auditing,
format support and transformation workflows
```

Both are useful.

---

# Online vs Offline Guessing

Critical distinction.

## Online

```text
Candidate
   ↓
Login Server
   ↓
Success / Failure
```

Risks:

```text
Account lockout
Rate limiting
Logs
Network traffic
```

## Offline

```text
Hash
   ↓
Local Password Auditor
   ↓
Candidate Verification
```

No repeated authentication request is required against the original login service.

Hashcat is primarily an:

```text
OFFLINE
```

password-auditing tool.

---

# Hashcat vs Hydra

```text
Hashcat
=
Offline password/hash auditing
```

```text
Hydra
=
Online authentication testing
```

Do not confuse them.

---

# Password Policy Evaluation

Authorized password audits can reveal:

```text
Weak passwords
Predictable patterns
Password reuse
Insufficient length
Organization-specific patterns
```

The objective is usually not:

```text
Recover as many passwords as possible
```

but:

```text
Measure credential resilience
and improve policy.
```

---

# Defensive Password Guidance

Prefer:

```text
Long passwords/passphrases
Unique credentials
Password managers
MFA
Rate limiting
Strong password hashing
Credential breach screening
```

For service accounts:

```text
Long random credentials
Automated rotation
Managed identities
Managed service accounts
```

---

# Ethical Handling

Recovered credentials are sensitive.

During an assessment:

```text
Minimize collection
Encrypt results
Restrict access
Avoid unnecessary screenshots
Do not reuse credentials elsewhere
Destroy according to engagement policy
```

---

# Password Audit Workflow

```text
Authorized Hash Material
        ↓
Identify Format
        ↓
Select Correct Mode
        ↓
Start Targeted Wordlist
        ↓
Apply Appropriate Rules
        ↓
Use Masks if justified
        ↓
Measure Results
        ↓
Report Password Weakness
```

Avoid beginning with an enormous brute-force search unless the known password policy makes it sensible.

---

# Efficiency Mindset

Generally prefer:

```text
Intelligence
   ↓
Targeted Candidates
```

over:

```text
Blind Exhaustive Search
```

Example:

```text
Known password policy
+
Organization-specific patterns
+
Controlled wordlist
```

can make an authorized audit more efficient and meaningful.

---

# Key Takeaway

Hashcat's core model is:

```text
HASH
  +
CANDIDATE
  ↓
HASH FUNCTION
  ↓
COMPARE
  ↓
MATCH?
```

It does not normally:

```text
Decrypt password hashes
```

Instead it performs:

```text
Candidate Generation
       +
Offline Verification
```

The main defensive lesson is:

```text
If password hashes are stolen,
password strength and the password
hashing algorithm become the final
line of defense.
```

Use:

```text
Strong Passwords
       +
Salt
       +
Modern Password KDF
       +
MFA
```

to reduce credential-compromise risk.
