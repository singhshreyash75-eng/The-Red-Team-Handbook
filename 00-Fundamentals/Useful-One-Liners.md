# ⚡ Useful Linux One-Liners

> A collection of practical Linux one-liners commonly used during CTFs, penetration testing, privilege escalation, and system enumeration.

---

# Overview

One-liners are short commands that perform powerful tasks.

Instead of writing scripts, they help you:

- Enumerate systems
- Search files
- Transfer files
- Analyze logs
- Discover secrets
- Perform recon
- Automate repetitive tasks

---

# Current User

```bash
whoami
```

Current UID

```bash
id
```

Current Shell

```bash
echo $SHELL
```

Hostname

```bash
hostname
```

OS Information

```bash
cat /etc/os-release
```

Kernel Version

```bash
uname -a
```

---

# Networking

IP Address

```bash
ip a
```

Routing Table

```bash
ip route
```

DNS Servers

```bash
cat /etc/resolv.conf
```

Open Ports

```bash
ss -tuln
```

Active Connections

```bash
netstat -tulnp
```

Ping

```bash
ping google.com
```

Traceroute

```bash
traceroute google.com
```

---

# File Enumeration

Find SUID Files

```bash
find / -perm -4000 2>/dev/null
```

Find SGID Files

```bash
find / -perm -2000 2>/dev/null
```

Find Writable Files

```bash
find / -writable 2>/dev/null
```

Find Hidden Files

```bash
find / -name ".*" 2>/dev/null
```

Find Config Files

```bash
find / -name "*.conf" 2>/dev/null
```

Find Backups

```bash
find / -name "*.bak" 2>/dev/null
```

---

# Searching

Search Password

```bash
grep -Ri "password" /
```

Search API Keys

```bash
grep -Ri "apikey" .
```

Search AWS Keys

```bash
grep -Ri "AKIA" .
```

Search SSH Keys

```bash
find / -name "id_rsa*" 2>/dev/null
```

---

# Users

List Users

```bash
cat /etc/passwd
```

List Groups

```bash
cat /etc/group
```

Logged In Users

```bash
w
```

Current Sessions

```bash
who
```

---

# Processes

Running Processes

```bash
ps aux
```

Top Processes

```bash
top
```

Kill Process

```bash
kill -9 PID
```

---

# Services

Running Services

```bash
systemctl list-units --type=service
```

Service Status

```bash
systemctl status apache2
```

---

# Logs

Authentication Logs

```bash
cat /var/log/auth.log
```

System Logs

```bash
journalctl
```

Live Logs

```bash
tail -f /var/log/syslog
```

---

# Downloads

Using wget

```bash
wget http://IP/file
```

Using curl

```bash
curl -O http://IP/file
```

Python Server

```bash
python3 -m http.server 8000
```

---

# File Transfer

Copy using SCP

```bash
scp file.txt user@IP:/tmp
```

Netcat Listener

```bash
nc -lvnp 4444
```

Netcat Connect

```bash
nc IP 4444
```

---

# Archives

Create ZIP

```bash
zip archive.zip file.txt
```

Extract ZIP

```bash
unzip archive.zip
```

Create TAR

```bash
tar -cvf files.tar folder
```

Extract TAR

```bash
tar -xvf files.tar
```

---

# Encoding

Base64 Encode

```bash
echo "admin" | base64
```

Base64 Decode

```bash
echo "YWRtaW4=" | base64 -d
```

SHA256

```bash
sha256sum file.txt
```

MD5

```bash
md5sum file.txt
```

---

# Permissions

Make Executable

```bash
chmod +x script.sh
```

Owner

```bash
chown user:user file
```

Permission

```bash
chmod 755 script.sh
```

---

# SSH

Generate Key

```bash
ssh-keygen
```

SSH Login

```bash
ssh user@IP
```

Copy Key

```bash
ssh-copy-id user@IP
```

---

# Git

Clone Repository

```bash
git clone URL
```

Status

```bash
git status
```

Pull

```bash
git pull
```

Push

```bash
git push
```

---

# Docker

Running Containers

```bash
docker ps
```

Images

```bash
docker images
```

Execute Shell

```bash
docker exec -it CONTAINER /bin/bash
```

---

# Nmap

Quick Scan

```bash
nmap IP
```

Version Detection

```bash
nmap -sV IP
```

Full Scan

```bash
nmap -A IP
```

All Ports

```bash
nmap -p- IP
```

---

# Privilege Escalation

Find SUID

```bash
find / -perm -4000 2>/dev/null
```

Find Writable Directories

```bash
find / -writable 2>/dev/null
```

Capabilities

```bash
getcap -r / 2>/dev/null
```

---

# Reverse Shell Helpers

Python

```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

TTY Upgrade

```bash
export TERM=xterm
```

Fix Shell

```bash
stty raw -echo
fg
reset
```

---

# Red Team Workflow

Basic Enumeration

```bash
whoami

id

hostname

ip a

ip route

cat /etc/passwd

ps aux

ss -tuln
```

---

# Common Mistakes

- Running destructive commands without checking the path.
- Ignoring hidden files.
- Forgetting stderr redirection (`2>/dev/null`).
- Not upgrading a reverse shell.
- Skipping basic enumeration.

---

# Interview Questions

- What is a one-liner?
- Why use `2>/dev/null`?
- Difference between `wget` and `curl`?
- Why is `python3 -m http.server` useful?
- How do you find SUID binaries?

---

# Quick Revision

- `whoami` → Current user
- `id` → UID/GID
- `ip a` → IP Address
- `ss -tuln` → Listening ports
- `find / -perm -4000` → SUID
- `ps aux` → Processes
- `wget` / `curl` → Downloads
- `python3 -m http.server` → Quick file server
- `ssh` → Remote login
- `nmap -sV` → Service detection

---

# Practice

- TryHackMe — Linux Fundamentals
- Hack The Box Academy — Linux Fundamentals
- GTFOBins
- OverTheWire Bandit

---

# References

- GTFOBins
- HackTricks
- Linux Documentation Project
- Hack The Box Academy
- TryHackMe
