# 🐚 Bash Fundamentals

> Bash (Bourne Again SHell) is the default shell on most Linux systems. For a Red Teamer, Bash is much more than a terminal—it is one of the most powerful automation tools available.

---

# Overview

Bash allows you to:

- Automate repetitive tasks
- Chain multiple commands together
- Write scripts
- Parse outputs
- Enumerate systems
- Build recon pipelines
- Automate privilege escalation checks

Almost every CTF, HTB machine, and real-world Linux server expects you to be comfortable with Bash.

---

# Why Bash Matters

Instead of running 20 commands manually, Bash allows you to execute them automatically.

Example:

Instead of

```bash
subfinder
httpx
nuclei
```

You can automate the entire workflow.

---

# Running Bash

Current shell

```bash
echo $SHELL
```

Start Bash

```bash
bash
```

Exit

```bash
exit
```

---

# Variables

Create variable

```bash
name="Shreyash"
```

Access variable

```bash
echo $name
```

Current user

```bash
echo $USER
```

Current directory

```bash
echo $PWD
```

Home directory

```bash
echo $HOME
```

---

# User Input

```bash
read name

echo $name
```

Example

```bash
read target

ping $target
```

---

# Comments

```bash
# This is a comment
```

---

# Command Substitution

```bash
today=$(date)

echo $today
```

Current directory

```bash
dir=$(pwd)

echo $dir
```

---

# If Statements

```bash
if [ -f notes.txt ]
then
    echo "Exists"
fi
```

Example

```bash
if ping -c 1 google.com
then
    echo "Internet Working"
else
    echo "Offline"
fi
```

---

# Comparison Operators

| Operator | Meaning |
|----------|---------|
| -eq | Equal |
| -ne | Not Equal |
| -gt | Greater Than |
| -lt | Less Than |
| -ge | Greater or Equal |
| -le | Less or Equal |

Example

```bash
if [ $a -eq 10 ]
```

---

# Loops

For Loop

```bash
for i in 1 2 3 4 5
do
    echo $i
done
```

Loop through files

```bash
for file in *.txt
do
    echo $file
done
```

---

# While Loop

```bash
count=1

while [ $count -le 5 ]
do
    echo $count
    count=$((count+1))
done
```

---

# Functions

```bash
hello(){

echo "Hello"

}

hello
```

With arguments

```bash
scan(){

nmap $1

}

scan 192.168.1.1
```

---

# Arrays

```bash
tools=("nmap" "ffuf" "nuclei")

echo ${tools[0]}

echo ${tools[1]}
```

Loop

```bash
for tool in "${tools[@]}"
do
    echo $tool
done
```

---

# Useful Operators

Append

```bash
>>
```

Overwrite

```bash
>
```

Pipe

```bash
|
```

AND

```bash
&&
```

OR

```bash
||
```

Background

```bash
&
```

---

# Pipes

Example

```bash
cat users.txt | grep admin
```

Count

```bash
cat users.txt | wc -l
```

Sort

```bash
cat names.txt | sort
```

Unique

```bash
cat users.txt | sort | uniq
```

---

# Redirection

Save output

```bash
ls > files.txt
```

Append

```bash
echo "admin" >> users.txt
```

Read input

```bash
sort < users.txt
```

---

# Useful Bash Commands

History

```bash
history
```

Aliases

```bash
alias ll="ls -la"
```

Environment variables

```bash
env
```

Export

```bash
export API_KEY=test
```

---

# Writing a Bash Script

Create file

```bash
nano script.sh
```

Example

```bash
#!/bin/bash

echo "Hello Red Team"
```

Run

```bash
bash script.sh
```

or

```bash
chmod +x script.sh

./script.sh
```

---

# Practical Scripts

## Ping Sweep

```bash
#!/bin/bash

for i in {1..254}
do
ping -c 1 192.168.1.$i &
done
```

---

## Directory Creator

```bash
#!/bin/bash

mkdir Recon

mkdir Enumeration

mkdir Exploitation

mkdir PrivEsc
```

---

## HTTP Status Checker

```bash
#!/bin/bash

read url

curl -I $url
```

---

# Bash in Red Teaming

Examples

- Recon automation
- Nmap automation
- FFUF automation
- Parsing Nmap XML
- Log analysis
- PrivEsc checks
- File transfers
- Reverse shell helpers

Example

```bash
subfinder -d example.com | httpx | nuclei
```

This entire workflow is powered by Bash pipelines.

---

# Common Mistakes

- Forgetting quotes around variables
- Running scripts without execute permissions
- Hardcoding sensitive data
- Ignoring exit codes
- Using `rm -rf` inside scripts without validation

---

# Interview Questions

### What is Bash?

### Difference between Bash and Shell?

### Difference between `>` and `>>`?

### Difference between `&&` and `||`?

### How do you make a script executable?

### What is a shebang?

---

# Quick Revision

- Bash is the default shell on most Linux systems.
- Variables start with `$`.
- `#!/bin/bash` is called the shebang.
- `&&` executes the next command only if the previous succeeds.
- `||` executes the next command if the previous fails.
- `|` pipes output to another command.
- `>` overwrites a file.
- `>>` appends to a file.
- `chmod +x` makes a script executable.

---

# Practice

TryHackMe

- Linux Fundamentals
- Bash Scripting

Hack The Box Academy

- Linux Fundamentals
- Bash Scripting

OverTheWire

- Bandit

---

# References

- GNU Bash Manual
- Linux Documentation Project
- Hack The Box Academy
- TryHackMe
