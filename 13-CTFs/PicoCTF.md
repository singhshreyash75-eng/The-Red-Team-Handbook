
# picoCTF

> Beginner-to-intermediate cybersecurity challenges covering cryptography, web security, reverse engineering, binary exploitation, and digital forensics.

## 1. What is picoCTF?

**picoCTF** is a cybersecurity education and Capture The Flag platform developed by Carnegie Mellon University's security education community.

Website: https://picoctf.org/

It focuses on learning through practical challenges.

Major categories:

```text
General Skills
Cryptography
Web Exploitation
Reverse Engineering
Binary Exploitation
Forensics
```

---

## 2. What is a CTF?

**CTF = Capture The Flag**

A CTF challenge requires discovering a hidden flag by solving a technical problem.

Example flag format:

```text
picoCTF{example_flag}
```

This is an illustrative format, not an actual challenge solution.

Conceptually:

```text
Challenge
    ↓
Analyze
    ↓
Solve
    ↓
Find Flag
    ↓
Submit
```

---

## 3. General Skills

General Skills challenges develop familiarity with:

```text
Linux
Terminal commands
File formats
Encoding
Networking
Programming
Basic scripting
```

Important commands:

```bash
file challenge.bin
strings challenge.bin
xxd challenge.bin
```

Check permissions:

```bash
ls -l
```

Inspect content:

```bash
cat file.txt
```

---

## 4. Encoding

Common formats:

```text
ASCII
Hexadecimal
Binary
Base64
URL Encoding
Unicode
```

Example Base64:

```text
aGVsbG8=
```

Decode:

```bash
printf 'aGVsbG8=' | base64 -d
```

Output:

```text
hello
```

Remember:

```text
Encoding != Encryption
```

---

## 5. Cryptography

Common CTF concepts:

```text
Caesar Cipher
ROT13
XOR
RSA
AES
Hashing
Modular Arithmetic
Frequency Analysis
```

Cryptography challenges often require understanding the mathematical construction rather than brute-forcing blindly.

---

## 6. Caesar Cipher

A Caesar cipher shifts alphabetic characters by a fixed amount.

Example:

```text
HELLO
  ↓
Shift +3
  ↓
KHOOR
```

It is a classical substitution cipher and is not secure by modern standards.

---

## 7. ROT13

ROT13 shifts letters by 13 positions.

Example:

```text
hello
  ↓
uryyb
```

Python:

```python
import codecs

print(codecs.decode("uryyb", "rot_13"))
```

---

## 8. XOR

XOR is common in introductory cryptography and reverse-engineering challenges.

Important property:

```text
A XOR B XOR B = A
```

Example:

```python
value = 42
key = 17

encoded = value ^ key
decoded = encoded ^ key

print(decoded)
```

Output:

```text
42
```

---

## 9. Web Exploitation

Typical topics:

```text
HTML Source
JavaScript
Cookies
HTTP Headers
Authentication
Input Validation
SQL Injection
XSS
```

Useful tools:

```text
Browser Developer Tools
Burp Suite
Caido
curl
```

---

## 10. Browser Developer Tools

Important tabs:

```text
Elements
Console
Network
Sources
Application
```

Inspect:

- HTML comments
- JavaScript files
- Network requests
- Cookies
- Client-side application behavior

Only test the challenge environment.

---

## 11. HTTP Requests

Inspect headers:

```bash
curl -I https://example.com
```

Retrieve page:

```bash
curl https://example.com
```

Understanding HTTP is essential for web-focused challenges.

---

## 12. Reverse Engineering

Reverse engineering involves analyzing a program to understand its behavior.

Common tools:

```text
Ghidra
GDB
strings
file
objdump
readelf
```

Basic workflow:

```text
Unknown Binary
      ↓
Identify Format
      ↓
Inspect Strings
      ↓
Analyze Functions
      ↓
Understand Logic
      ↓
Recover Flag
```

---

## 13. Binary Inspection

Determine file type:

```bash
file program
```

Extract printable strings:

```bash
strings program
```

ELF headers:

```bash
readelf -h program
```

Disassemble:

```bash
objdump -d program
```

---

## 14. GDB

**GDB = GNU Debugger**

Start:

```bash
gdb ./program
```

Common debugger commands:

```text
break main
run
next
step
continue
info registers
backtrace
```

Use GDB to understand execution and program state.

---

## 15. Binary Exploitation

Common concepts:

```text
Stack
Heap
Registers
Memory Layout
Buffer Overflow
Format Strings
ASLR
NX
Stack Canaries
```

A typical binary challenge requires identifying how input affects program memory or control flow.

---

## 16. Forensics

Digital forensics challenges may involve:

```text
Images
PCAP files
Archives
Metadata
Logs
Memory dumps
Disk images
```

Important tools:

```text
Wireshark
ExifTool
binwalk
strings
file
xxd
```

---

## 17. Metadata Analysis

Example:

```bash
exiftool image.jpg
```

Useful for identifying:

```text
Creation metadata
Software
File characteristics
Embedded metadata
```

Metadata may be absent or intentionally misleading.

---

## 18. PCAP Analysis

Open packet captures in Wireshark.

Useful filters:

```text
http
dns
tcp
udp
icmp
```

Conceptually:

```text
PCAP
 ↓
Protocol Analysis
 ↓
Interesting Conversation
 ↓
Challenge Evidence
```

---

## 19. Steganography

Steganography involves hiding information within another medium.

Possible carriers:

```text
Images
Audio
Video
Documents
```

Common investigation methods:

```text
Metadata inspection
File signatures
Embedded file detection
Bit-level analysis
```

Do not assume every image challenge uses steganography.

---

## 20. Python for CTFs

Python is particularly useful for:

```text
Encoding
Decoding
Cryptographic calculations
File parsing
Automation
Binary data manipulation
```

Example:

```python
data = bytes.fromhex("68656c6c6f")

print(data.decode())
```

Output:

```text
hello
```

---

## 21. Useful Commands

```bash
file challenge
strings challenge
xxd challenge
sha256sum challenge
```

Extract ZIP:

```bash
unzip archive.zip
```

Extract tar:

```bash
tar -xf archive.tar
```

---

## 22. Challenge-Solving Methodology

```text
Read Description
      ↓
Identify Category
      ↓
Inspect Provided Files
      ↓
Form Hypothesis
      ↓
Test Hypothesis
      ↓
Extract Evidence
      ↓
Find Flag
      ↓
Document Solution
```

---

## 23. When Stuck

Use a structured approach:

```text
Re-read challenge
Check file format
Inspect metadata
Review assumptions
Try smaller experiments
Research relevant concepts
```

Avoid immediately opening a full walkthrough.

Hints are more useful when they help identify the next concept to investigate.

---

## 24. Writeup Template

```text
Challenge Name:
__________

Category:
__________

Difficulty:
__________

Objective:
__________

Initial Observations:
__________

Tools Used:
__________

Solution Approach:
__________

Key Concept:
__________

Lessons Learned:
__________
```

---

## Key Takeaway

picoCTF builds foundational cybersecurity problem-solving skills.

```text
OBSERVE
   ↓
REASON
   ↓
EXPERIMENT
   ↓
VERIFY
   ↓
DOCUMENT
```

Focus on understanding the underlying technique rather than memorizing challenge-specific flags.
