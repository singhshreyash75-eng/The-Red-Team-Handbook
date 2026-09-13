# Bash

> Bash scripting fundamentals for Linux automation, reconnaissance workflows, file processing, and security tooling.

## What is Bash?

**Bash = Bourne Again Shell**

Bash is both:

```text
Command-line shell
        +
Scripting language
```

It is widely used on:

```text
Linux
Unix-like systems
macOS environments
Security distributions
Servers
```

In security work, Bash is especially useful for:

```text
Automation
Tool chaining
Recon pipelines
File processing
Log analysis
System enumeration
```

---

# Shell vs Script

Interactive shell:

```bash
whoami
pwd
ls
```

Script:

```bash
#!/bin/bash

whoami
pwd
ls
```

A script stores multiple commands so they can be executed repeatedly.

---

# Shebang

A Bash script commonly begins with:

```bash
#!/bin/bash
```

or:

```bash
#!/usr/bin/env bash
```

This tells the operating system which interpreter should execute the file.

---

# Create a Script

Example:

```bash
nano script.sh
```

Content:

```bash
#!/bin/bash

echo "Hello"
whoami
```

Make executable:

```bash
chmod +x script.sh
```

Run:

```bash
./script.sh
```

Alternatively:

```bash
bash script.sh
```

---

# Variables

Create:

```bash
name="Alice"
```

Use:

```bash
echo "$name"
```

Important:

```bash
name="Alice"
```

is correct.

Avoid spaces around `=`:

```text
name = "Alice"
```

because Bash interprets that differently.

---

# Variable Expansion

Example:

```bash
domain="example.com"

echo "Target: $domain"
```

Output:

```text
Target: example.com
```

Prefer:

```bash
"$variable"
```

when passing variables containing arbitrary strings to commands.

---

# Command Substitution

Store command output:

```bash
user=$(whoami)
```

Then:

```bash
echo "$user"
```

Another example:

```bash
date_now=$(date)
```

Conceptually:

```text
Command
   ↓
Output
   ↓
Variable
```

---

# User Input

Read input:

```bash
read -r name
```

Example:

```bash
#!/bin/bash

echo "Enter domain:"
read -r domain

echo "Target: $domain"
```

---

# Script Arguments

Suppose:

```bash
./script.sh example.com
```

Inside the script:

```text
$0 = script name
$1 = first argument
$2 = second argument
$# = number of arguments
$@ = all arguments
```

Example:

```bash
#!/bin/bash

domain="$1"

echo "Target: $domain"
```

Run:

```bash
./script.sh example.com
```

---

# Exit Status

Linux commands return an exit status.

Convention:

```text
0
=
Success
```

Non-zero:

```text
Error / different condition
```

Check the previous command:

```bash
echo $?
```

This is extremely useful for scripting logic.

---

# Conditional Statements

Basic structure:

```bash
if condition; then
    command
fi
```

Example:

```bash
if [ -f "targets.txt" ]; then
    echo "File exists"
fi
```

---

# If / Else

```bash
if [ -f "targets.txt" ]; then
    echo "Found"
else
    echo "Missing"
fi
```

---

# File Tests

Useful tests:

```text
-f    regular file exists
-d    directory exists
-e    path exists
-r    readable
-w    writable
-x    executable
```

Example:

```bash
if [ -d "/tmp" ]; then
    echo "Directory exists"
fi
```

---

# String Comparison

Example:

```bash
if [ "$user" = "root" ]; then
    echo "Running as root"
fi
```

Always quote variables unless you specifically need different shell expansion behavior.

---

# Numeric Comparison

Common operators:

```text
-eq    equal
-ne    not equal
-gt    greater than
-lt    less than
-ge    greater/equal
-le    less/equal
```

Example:

```bash
count=10

if [ "$count" -gt 5 ]; then
    echo "Greater than five"
fi
```

---

# For Loop

Example:

```bash
for item in one two three; do
    echo "$item"
done
```

Output:

```text
one
two
three
```

---

# Loop Through Files

```bash
for file in *.txt; do
    echo "$file"
done
```

Useful for batch processing.

---

# While Loop

```bash
count=1

while [ "$count" -le 5 ]; do
    echo "$count"
    count=$((count + 1))
done
```

---

# Reading a File Line-by-Line

Safe basic pattern:

```bash
while IFS= read -r line; do
    echo "$line"
done < targets.txt
```

This is generally safer than:

```bash
for line in $(cat targets.txt)
```

because whitespace is preserved correctly.

---

# Functions

Functions make scripts reusable.

```bash
greet() {
    echo "Hello $1"
}
```

Call:

```bash
greet "Alice"
```

---

# Arrays

Example:

```bash
tools=("nmap" "httpx" "nuclei")
```

Access:

```bash
echo "${tools[0]}"
```

All values:

```bash
echo "${tools[@]}"
```

Loop:

```bash
for tool in "${tools[@]}"; do
    echo "$tool"
done
```

---

# Pipes

The pipe:

```text
|
```

passes one command's standard output to another command's standard input.

Example:

```bash
cat domains.txt | sort -u
```

Conceptually:

```text
Command A
   ↓
stdout
   ↓
Command B
```

---

# Security Recon Pipelines

Bash is especially useful for chaining tools.

Example conceptual pipeline:

```text
Subfinder
    ↓
Subdomains
    ↓
httpx
    ↓
Live Hosts
```

Shell form:

```bash
subfinder -d example.com -silent | httpx
```

For authorized scope only.

---

# Output Redirection

Overwrite:

```bash
command > output.txt
```

Append:

```bash
command >> output.txt
```

Errors:

```bash
command 2> errors.txt
```

Both stdout and stderr:

```bash
command > output.txt 2>&1
```

---

# /dev/null

Discard output:

```bash
command > /dev/null
```

Discard errors:

```bash
command 2>/dev/null
```

Useful when irrelevant permission errors create noise.

---

# grep

Search text:

```bash
grep "admin" file.txt
```

Case insensitive:

```bash
grep -i "admin" file.txt
```

Regex:

```bash
grep -E 'api|admin|login' urls.txt
```

Recursive:

```bash
grep -R "password" .
```

Use sensitive-data searches only where authorized.

---

# sort

Sort:

```bash
sort file.txt
```

Unique:

```bash
sort -u file.txt
```

This is extremely useful in recon pipelines.

Example:

```bash
cat tool1.txt tool2.txt | sort -u > combined.txt
```

---

# uniq

Remove adjacent duplicates:

```bash
uniq file.txt
```

Usually combine with sorting:

```bash
sort file.txt | uniq
```

or simply:

```bash
sort -u file.txt
```

---

# cut

Extract fields.

Example:

```bash
cut -d ':' -f 1 /etc/passwd
```

Meaning:

```text
Delimiter:
:

Field:
1
```

---

# awk

`awk` is useful for structured text processing.

Example:

```bash
awk '{print $1}' file.txt
```

Print multiple fields:

```bash
awk '{print $1, $3}' file.txt
```

---

# sed

`sed` performs stream editing.

Example replacement:

```bash
sed 's/http:/https:/g' urls.txt
```

Conceptually:

```text
Input
 ↓
Pattern Transformation
 ↓
Output
```

---

# find

Search filesystem:

```bash
find . -name "*.txt"
```

Files only:

```bash
find . -type f
```

Directories:

```bash
find . -type d
```

---

# xargs

`xargs` converts input into command arguments.

Example:

```bash
cat domains.txt | xargs -n 1 echo
```

Useful when a command does not naturally consume stdin.

Be careful with filenames/values containing whitespace or special characters.

---

# curl

`curl` is frequently used inside Bash scripts for HTTP/API interaction.

Example:

```bash
curl https://example.test
```

Headers:

```bash
curl -I https://example.test
```

Save:

```bash
curl -o page.html https://example.test
```

---

# Environment Variables

View:

```bash
env
```

Access:

```bash
echo "$HOME"
echo "$PATH"
```

Create for current shell:

```bash
export NAME="value"
```

Environment variables can contain secrets, so avoid exposing them unnecessarily in logs.

---

# PATH

`PATH` determines where the shell searches for executables.

```bash
echo "$PATH"
```

Conceptually:

```text
Command
   ↓
Search PATH directories
   ↓
Executable
```

Unsafe PATH handling can create security problems in privileged scripts.

---

# Quoting

Three important forms:

```text
"double quotes"
'single quotes'
unquoted
```

Double quotes allow variable expansion:

```bash
echo "$USER"
```

Single quotes treat contents literally:

```bash
echo '$USER'
```

Output:

```text
$USER
```

Correct quoting is essential for reliable and secure shell scripting.

---

# Command Injection Risk

Dangerous scripting pattern:

```text
Untrusted Input
      ↓
Construct Shell Command
      ↓
Shell Interpretation
```

Avoid passing untrusted strings into constructs such as:

```bash
eval
```

without strict justification and validation.

Prefer structured arguments and quote variables correctly.

---

# set Options

Useful defensive scripting options:

```bash
set -e
```

Exit on many command failures.

```bash
set -u
```

Treat unset variables as errors.

A commonly seen strict-mode style is:

```bash
set -euo pipefail
```

But understand its behavior before applying it blindly because error semantics can be subtle.

---

# Comments

```bash
# This is a comment
```

Use comments to explain:

```text
Why something is done
```

rather than merely repeating the command.

---

# Simple Recon Script

Example for an authorized lab:

```bash
#!/usr/bin/env bash

domain="$1"

if [ -z "$domain" ]; then
    echo "Usage: $0 <domain>"
    exit 1
fi

echo "[+] Target: $domain"

subfinder -d "$domain" -silent > subdomains.txt

sort -u subdomains.txt -o subdomains.txt

httpx -l subdomains.txt -o live.txt

echo "[+] Done"
```

Conceptual flow:

```text
Domain
 ↓
Subfinder
 ↓
Deduplicate
 ↓
httpx
 ↓
Live Hosts
```

---

# Why Bash Matters in Security

Many security tools follow:

```text
Input
 ↓
Process
 ↓
Output
```

Bash makes them composable:

```text
Tool A
 ↓
Tool B
 ↓
grep
 ↓
sort
 ↓
File
```

This enables rapid workflow automation.

---

# Key Takeaway

Bash scripting is fundamentally:

```text
COMMANDS
   +
VARIABLES
   +
CONDITIONS
   +
LOOPS
   +
FUNCTIONS
   +
PIPES
   +
TEXT PROCESSING
```

For security work, focus particularly on:

```text
Tool chaining
File processing
Output normalization
Automation
Error handling
Correct quoting
```

The goal is not to replace larger programming languages.

Use Bash when the task is primarily:

```text
"Run commands and connect their outputs."
```
