# 🐧 Linux Basics

> Linux is the foundation of modern cybersecurity. Most servers, cloud infrastructure, CTF environments, and penetration testing distributions run Linux. A strong understanding of Linux is essential for every Red Teamer.

---

# Learning Objectives

After completing this note, you should be able to:

- Understand Linux fundamentals
- Navigate the Linux filesystem
- Work with files and directories
- Understand users and permissions
- Execute essential Linux commands
- Be comfortable using Kali Linux during CTFs and pentests

---

# What is Linux?

Linux is an **open-source Unix-like operating system kernel** created by **Linus Torvalds** in 1991.

Popular distributions include:

- Kali Linux
- Ubuntu
- Debian
- Fedora
- Arch Linux
- Parrot OS
- BlackArch

For offensive security, Kali Linux and Parrot OS are the most commonly used distributions.

---

# Linux File System

Unlike Windows, Linux doesn't use drive letters like `C:\` or `D:\`.

Everything begins at the root directory:

```text
/
```

Example:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── opt
├── proc
├── root
├── run
├── sbin
├── tmp
├── usr
└── var
```

---

# Important Directories

| Directory | Purpose |
|------------|---------|
| `/` | Root directory |
| `/home` | User home directories |
| `/root` | Root user's home directory |
| `/etc` | Configuration files |
| `/bin` | Essential binaries |
| `/usr` | User applications |
| `/var` | Logs, cache, mail |
| `/tmp` | Temporary files |
| `/dev` | Device files |
| `/proc` | Running process information |

---

# Users

Linux supports multiple users.

Examples:

```text
root
kali
ubuntu
john
```

Current user:

```bash
whoami
```

User information:

```bash
id
```

Logged in users:

```bash
who

w
```

---

# Root User

The root user has unrestricted access.

Become root:

```bash
sudo -i
```

or

```bash
sudo su
```

⚠️ Never execute unknown commands as root.

---

# Navigation

Current directory

```bash
pwd
```

List files

```bash
ls
```

Detailed listing

```bash
ls -l
```

Hidden files

```bash
ls -la
```

Change directory

```bash
cd DirectoryName

cd ..

cd ~

cd /

cd -
```

---

# Working with Files

Create file

```bash
touch notes.txt
```

Create directory

```bash
mkdir Notes
```

Nested directories

```bash
mkdir -p Labs/HTB/Linux
```

Copy file

```bash
cp file.txt backup.txt
```

Move or rename

```bash
mv old.txt new.txt
```

Delete file

```bash
rm file.txt
```

Delete directory

```bash
rm -r FolderName
```

⚠️ Dangerous

```bash
rm -rf FolderName
```

---

# Viewing Files

Display contents

```bash
cat file.txt
```

View page by page

```bash
less file.txt
```

First 10 lines

```bash
head file.txt
```

Last 10 lines

```bash
tail file.txt
```

Live logs

```bash
tail -f logfile.log
```

---

# Editing Files

Nano

```bash
nano file.txt
```

Vim

```bash
vim file.txt
```

---

# Searching

Find files

```bash
find / -name passwd
```

Search text

```bash
grep "admin" users.txt
```

Recursive search

```bash
grep -r "password" .
```

---

# Processes

View processes

```bash
ps aux
```

Interactive process viewer

```bash
top
```

or

```bash
htop
```

Kill process

```bash
kill PID
```

Force kill

```bash
kill -9 PID
```

---

# Networking Commands

IP Address

```bash
ip a
```

Routing table

```bash
ip route
```

Ping

```bash
ping google.com
```

Download file

```bash
wget URL
```

or

```bash
curl URL
```

---

# Package Management

Debian / Ubuntu / Kali

```bash
sudo apt update

sudo apt upgrade

sudo apt install nmap
```

Fedora

```bash
sudo dnf install
```

Arch

```bash
sudo pacman -S
```

---

# Useful Shortcuts

| Shortcut | Action |
|-----------|--------|
| Ctrl + C | Stop current process |
| Ctrl + Z | Suspend process |
| Ctrl + D | Logout |
| Ctrl + L | Clear terminal |
| Tab | Auto-complete |
| ↑ | Previous command |
| history | Command history |

---

# Practical Exercise

Run the following:

```bash
mkdir LinuxLab

cd LinuxLab

touch notes.txt

echo "Hello Red Team" > notes.txt

cat notes.txt

mkdir Recon

mv notes.txt Recon/

cd Recon

pwd

ls -la
```

---

# Common Beginner Mistakes

- Using `rm -rf` without checking the path
- Running every command as root
- Forgetting relative vs absolute paths
- Ignoring hidden files (`ls -la`)
- Not checking permissions before executing scripts

---

# Interview Questions

### What is Linux?

### Difference between Linux and Unix?

### What is the root user?

### Difference between absolute and relative paths?

### Difference between `cp` and `mv`?

### Difference between `grep` and `find`?

### What does `sudo` do?

---

# Quick Revision

- Linux is a Unix-like operating system.
- Everything starts from `/`.
- `/etc` stores configuration files.
- `/home` stores user data.
- `pwd` prints the current directory.
- `ls -la` lists hidden files.
- `cp` copies files.
- `mv` moves or renames files.
- `grep` searches inside files.
- `find` searches for files.
- `sudo` executes commands with elevated privileges.

---

# Practice

- TryHackMe — Linux Fundamentals
- Hack The Box Academy — Linux Fundamentals
- OverTheWire — Bandit

---

# References

- Linux Documentation Project
- Ubuntu Documentation
- Kali Linux Documentation
- GTFOBins
