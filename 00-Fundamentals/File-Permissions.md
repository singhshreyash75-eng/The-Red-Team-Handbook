# 🔐 Linux File Permissions

> File permissions control who can read, write, and execute files on a Linux system. Understanding permissions is essential for privilege escalation, system administration, and penetration testing.

---

# Overview

Linux is a multi-user operating system.

Every file and directory has permissions that determine:

- Who can read it
- Who can modify it
- Who can execute it

Improper permissions are one of the most common privilege escalation vectors.

---

# Learning Objectives

After reading this chapter, you should understand:

- File ownership
- Permission types
- chmod
- chown
- Numeric permissions
- Symbolic permissions
- SUID
- SGID
- Sticky Bit
- Red Team Perspective

---

# File Ownership

Every file belongs to:

```
Owner

↓

Group

↓

Others
```

Example

```bash
-rwxr-xr-- 1 kali kali notes.sh
```

```
Owner → kali

Group → kali

Others → Everyone else
```

---

# Permission Types

Linux has three basic permissions.

| Permission | Symbol | Value |
|------------|--------|------:|
| Read | r | 4 |
| Write | w | 2 |
| Execute | x | 1 |

---

# Read Permission (r)

Allows viewing file contents.

Example

```bash
cat file.txt
```

Without read permission:

```
Permission denied
```

---

# Write Permission (w)

Allows modifying a file.

Example

```bash
echo "Hello" >> file.txt
```

---

# Execute Permission (x)

Allows execution.

Example

```bash
./script.sh
```

Without execute permission:

```
Permission denied
```

---

# Viewing Permissions

```bash
ls -l
```

Example

```bash
-rwxr-xr-x
```

Breakdown

```
-

↓

File

rwx

↓

Owner

r-x

↓

Group

r-x

↓

Others
```

---

# Numeric Permissions

Each permission has a value.

```
Read = 4

Write = 2

Execute = 1
```

Examples

| Number | Meaning |
|---------|---------|
|777|Everyone Full Access|
|755|Owner Full, Others Read & Execute|
|744|Owner Full, Others Read|
|700|Owner Only|
|644|Common File Permission|
|600|Private File|

---

# chmod

Change permissions.

Example

```bash
chmod 755 script.sh
```

Make executable

```bash
chmod +x script.sh
```

Remove execute

```bash
chmod -x script.sh
```

Read only

```bash
chmod 444 file.txt
```

---

# Symbolic Mode

Owner

```bash
chmod u+x script.sh
```

Group

```bash
chmod g+w file.txt
```

Others

```bash
chmod o-r file.txt
```

Everyone

```bash
chmod a+x script.sh
```

---

# chown

Change file owner.

Example

```bash
sudo chown user file.txt
```

Owner and Group

```bash
sudo chown user:group file.txt
```

---

# chgrp

Change group.

```bash
sudo chgrp developers file.txt
```

---

# SUID

Special permission.

When set, a file runs with the permissions of its owner.

Example

```bash
-rwsr-xr-x
```

Find SUID files

```bash
find / -perm -4000 2>/dev/null
```

This is one of the first commands used during Linux PrivEsc.

---

# SGID

Special Group ID.

```
-rwxr-sr-x
```

Files execute with group permissions.

Directories inherit group ownership.

---

# Sticky Bit

Usually found on

```
/tmp
```

Example

```
drwxrwxrwt
```

Only the file owner can delete their files.

---

# umask

Controls default permissions.

View current value

```bash
umask
```

Example

```
022
```

---

# Useful Commands

View permissions

```bash
ls -l
```

Recursive permissions

```bash
chmod -R 755 folder
```

Find writable files

```bash
find / -writable 2>/dev/null
```

Find SUID

```bash
find / -perm -4000 2>/dev/null
```

Find SGID

```bash
find / -perm -2000 2>/dev/null
```

Find world writable

```bash
find / -perm -2 2>/dev/null
```

---

# Red Team Perspective

During Linux privilege escalation always check:

- Writable directories
- Writable scripts
- Writable cron jobs
- SUID binaries
- SGID binaries
- World writable files
- Incorrect ownership

Tools

- LinPEAS
- GTFOBins
- pspy

---

# Common Mistakes

- Giving files 777 permissions
- Running unknown SUID binaries
- Forgetting execute permissions
- Ignoring world writable files

---

# Interview Questions

- Difference between 755 and 777?
- What does chmod do?
- What is chown?
- What is SUID?
- What is SGID?
- What is the Sticky Bit?
- Why is 777 dangerous?

---

# Quick Revision

- Read = 4
- Write = 2
- Execute = 1
- chmod changes permissions
- chown changes owner
- SUID executes as owner
- SGID executes as group
- Sticky Bit protects shared directories
- Always check SUID during PrivEsc

---

# Practice

TryHackMe

- Linux PrivEsc

Hack The Box Academy

- Linux Privilege Escalation

GTFOBins

- https://gtfobins.github.io/

---

# References

- Linux Documentation Project
- GTFOBins
- Hack The Box Academy
- TryHackMe
