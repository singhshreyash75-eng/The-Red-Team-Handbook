# 🔍 Regular Expressions (Regex)

> Regular Expressions (Regex) are patterns used to search, match, extract, and manipulate text. Regex is widely used in cybersecurity for log analysis, payload filtering, reconnaissance, parsing outputs, and automation.

---

# Overview

Regex allows you to search text using patterns instead of exact words.

Instead of searching for

```
admin
```

Regex can search for

```
admin1
admin123
administrator
```

using a single pattern.

---

# Learning Objectives

After this chapter you should understand:

- What Regex is
- Common Regex Symbols
- Character Classes
- Quantifiers
- Anchors
- Groups
- Practical Examples
- Red Team Perspective

---

# Why Regex Matters

Regex is commonly used in:

- grep
- Burp Suite
- Nuclei
- ffuf
- Python
- Bash
- Log Analysis
- SIEM
- IDS Rules
- WAF Rules

---

# Basic Syntax

| Symbol | Meaning |
|---------|---------|
| . | Any character |
| * | Zero or more |
| + | One or more |
| ? | Zero or one |
| ^ | Beginning of line |
| $ | End of line |
| [] | Character class |
| () | Group |
| \ | Escape character |

---

# Wildcard

```
.
```

Matches any single character.

Example

```
a.c
```

Matches

```
abc

axc

a9c
```

---

# Character Classes

```
[abc]
```

Matches

```
a

b

c
```

Range

```
[a-z]
```

Uppercase

```
[A-Z]
```

Numbers

```
[0-9]
```

Alphanumeric

```
[a-zA-Z0-9]
```

---

# Quantifiers

Zero or more

```
*
```

One or more

```
+
```

Optional

```
?
```

Exact

```
{5}
```

Range

```
{2,5}
```

---

# Anchors

Beginning

```
^admin
```

Matches

```
admin123
```

End

```
password$
```

Matches

```
my_password
```

---

# Common Character Classes

Digit

```
\d
```

Not Digit

```
\D
```

Word

```
\w
```

Not Word

```
\W
```

Whitespace

```
\s
```

Not Whitespace

```
\S
```

---

# Groups

Example

```
(admin|root)
```

Matches

```
admin

root
```

---

# Practical Examples

Email

```regex
^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$
```

IPv4

```regex
(\d{1,3}\.){3}\d{1,3}
```

Domain

```regex
[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
```

Numbers

```regex
\d+
```

Hexadecimal

```regex
[0-9a-fA-F]+
```

---

# grep with Regex

Search numbers

```bash
grep "[0-9]" file.txt
```

Search emails

```bash
grep -E "[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+" users.txt
```

---

# Regex in Burp Suite

Common uses:

- Match responses
- Find JWTs
- Extract Emails
- Find API Keys
- Session Tokens
- Secrets

---

# Regex in Nuclei

Example

```yaml
regex:
  - "admin"
```

---

# Regex in Bash

```bash
echo "admin123" | grep -E "admin[0-9]+"
```

---

# Regex in Python

```python
import re

text = "admin123"

match = re.search(r"admin\d+", text)

print(match.group())
```

---

# Red Team Perspective

Regex helps during:

- Log Analysis
- Parsing Nmap Output
- Password Discovery
- Secret Hunting
- API Key Detection
- AWS Key Detection
- Email Collection
- Burp Match & Replace
- Nuclei Templates

---

# Common Beginner Mistakes

- Forgetting to escape special characters
- Confusing `*` with `+`
- Using greedy patterns
- Forgetting anchors (`^` and `$`)

---

# Interview Questions

- What is Regex?
- Difference between `*` and `+`?
- What does `.` match?
- What is `\d`?
- Difference between `^` and `$`?

---

# Quick Revision

- `.` → Any character
- `*` → Zero or more
- `+` → One or more
- `?` → Optional
- `^` → Start
- `$` → End
- `\d` → Digit
- `\w` → Word character
- `[]` → Character class
- `()` → Group

---

# Practice

- regex101.com
- regexr.com
- PortSwigger Academy
- TryHackMe

---

# References

- Regular-Expressions.info
- Regex101 Documentation
- Python re Module
