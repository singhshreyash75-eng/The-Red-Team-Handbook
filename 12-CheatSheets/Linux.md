
# Linux — Cheat Sheet

> Essential Linux commands for system administration, enumeration, troubleshooting, and authorized security assessments.

## 1. System Information

Current user:

```bash
whoami
id
```

Hostname:

```bash
hostname
hostnamectl
```

Operating system:

```bash
cat /etc/os-release
```

Kernel:

```bash
uname -a
uname -r
```

System uptime:

```bash
uptime
```

Architecture:

```bash
uname -m
```

---

## 2. Filesystem Navigation

```bash
pwd                  # Current directory
ls                   # List files
ls -la               # Detailed listing
cd /path             # Change directory
cd ..                # Parent directory
cd ~                 # Home directory
tree                 # Directory tree
```

Important directories:

```text
/etc       System configuration
/var/log   Logs
/home      User directories
/root      Root home
/tmp       Temporary files
/proc      Process/kernel information
/dev       Device files
/usr       Programs and libraries
```

---

## 3. File Operations

```bash
touch file.txt
mkdir directory
mkdir -p parent/child

cp file.txt copy.txt
mv old.txt new.txt
rm file.txt

cat file.txt
less file.txt
head file.txt
tail file.txt
```

Follow a log:

```bash
tail -f /var/log/syslog
```

Recursive removal is destructive:

```bash
rm -r directory
```

Use carefully.

---

## 4. File Permissions

View:

```bash
ls -l
```

Example:

```text
-rwxr-xr--
```

Breakdown:

```text
Owner: rwx
Group: r-x
Others: r--
```

Numeric permissions:

```text
r = 4
w = 2
x = 1
```

Examples:

```bash
chmod 755 script.sh
chmod 644 file.txt
chmod 600 secret.txt
```

Change ownership:

```bash
sudo chown user:group file.txt
```

---

## 5. Users and Groups

```bash
whoami
id
groups
who
w
```

User information:

```bash
getent passwd
```

Current UID:

```bash
id -u
```

Current GID:

```bash
id -g
```

List group information:

```bash
getent group
```

---

## 6. sudo

Check permitted administrative commands:

```bash
sudo -l
```

Run an authorized administrative command:

```bash
sudo systemctl status ssh
```

Important:

```text
sudo permissions
!=
Automatically unrestricted root access
```

Actual privileges depend on sudoers configuration.

---

## 7. Process Enumeration

```bash
ps aux
ps -ef
top
htop
```

Find processes:

```bash
pgrep -a ssh
```

Process tree:

```bash
pstree -p
```

Terminate a process you own:

```bash
kill <PID>
```

Force termination:

```bash
kill -9 <PID>
```

Prefer graceful termination first.

---

## 8. Services

Systemd:

```bash
systemctl list-units --type=service
```

Check service:

```bash
systemctl status ssh
```

View enabled services:

```bash
systemctl list-unit-files --type=service
```

Service logs:

```bash
journalctl -u ssh
```

Recent logs:

```bash
journalctl -xe
```

---

## 9. Networking

Interfaces:

```bash
ip addr
```

Routes:

```bash
ip route
```

Listening sockets:

```bash
ss -tuln
```

Sockets with processes:

```bash
sudo ss -tulpn
```

Connectivity:

```bash
ping -c 4 192.0.2.10
```

DNS:

```bash
dig example.com
```

HTTP headers:

```bash
curl -I https://example.com
```

---

## 10. Filesystem Search

Find files:

```bash
find /home -type f
```

Find by name:

```bash
find /etc -name "*.conf"
```

Find directories:

```bash
find /var -type d
```

Find recently modified files:

```bash
find /var/log -type f -mtime -1
```

Find files larger than 100 MB:

```bash
find /home -type f -size +100M
```

---

## 11. SUID and SGID

SUID files execute with the file owner's effective UID.

Find SUID files:

```bash
find /usr /bin /sbin -type f -perm -4000 2>/dev/null
```

Find SGID files:

```bash
find /usr /bin /sbin -type f -perm -2000 2>/dev/null
```

Security review:

```text
Unexpected SUID binary
      ↓
Inspect ownership
      ↓
Verify necessity
      ↓
Check package provenance
```

SUID presence alone does not prove a vulnerability.

---

## 12. Linux Capabilities

List capabilities on a binary:

```bash
getcap /usr/bin/ping
```

Search common executable directories:

```bash
getcap -r /usr/bin /usr/local/bin 2>/dev/null
```

Important capabilities:

```text
CAP_SETUID
CAP_SETGID
CAP_NET_ADMIN
CAP_SYS_ADMIN
CAP_DAC_OVERRIDE
```

Unexpected capabilities can expand a program's effective privileges.

---

## 13. Cron Jobs

System cron:

```bash
cat /etc/crontab
```

Current user's crontab:

```bash
crontab -l
```

Cron directories:

```bash
ls -la /etc/cron.*
```

Review:

```text
Scheduled command
Executing user
Script ownership
File permissions
Writable directories
```

---

## 14. Environment Variables

```bash
env
printenv
echo "$PATH"
echo "$HOME"
echo "$SHELL"
```

Security-sensitive variables may include:

```text
API tokens
Database credentials
Cloud credentials
Proxy configuration
```

Avoid exposing secret values in logs.

---

## 15. Log Analysis

Common paths:

```text
/var/log/auth.log
/var/log/syslog
/var/log/messages
/var/log/journal
```

Availability varies by distribution.

Search authentication failures:

```bash
grep -i "failed" /var/log/auth.log
```

Systemd:

```bash
journalctl --since "1 hour ago"
```

---

## 16. Text Processing

Search:

```bash
grep "error" app.log
```

Case insensitive:

```bash
grep -i "error" app.log
```

Unique values:

```bash
sort -u hosts.txt
```

Count lines:

```bash
wc -l hosts.txt
```

Extract field:

```bash
cut -d ':' -f 1 /etc/passwd
```

AWK:

```bash
awk '{print $1}' file.txt
```

Replace text:

```bash
sed 's/old/new/g' file.txt
```

---

## 17. Disk and Memory

Disk usage:

```bash
df -h
```

Directory size:

```bash
du -sh /var/log
```

Memory:

```bash
free -h
```

Block devices:

```bash
lsblk
```

Mounted filesystems:

```bash
findmnt
```

---

## 18. Package Management

Debian/Ubuntu:

```bash
sudo apt update
apt list --installed
```

Fedora/RHEL:

```bash
dnf list installed
```

Installed package versions help determine patch status.

---

## 19. SSH

Connect:

```bash
ssh user@192.0.2.10
```

Custom port:

```bash
ssh -p 2222 user@192.0.2.10
```

Copy file:

```bash
scp file.txt user@192.0.2.10:/tmp/
```

Generate key:

```bash
ssh-keygen -t ed25519
```

Protect private keys:

```bash
chmod 600 ~/.ssh/id_ed25519
```

---

## 20. Archives

Create:

```bash
tar -czf archive.tar.gz folder/
```

Extract:

```bash
tar -xzf archive.tar.gz
```

ZIP:

```bash
zip -r archive.zip folder/
```

---

## 21. Linux Security Enumeration Workflow

```text
Current Identity
      ↓
OS / Kernel
      ↓
Users and Groups
      ↓
sudo Permissions
      ↓
Processes and Services
      ↓
Network Listeners
      ↓
File Permissions
      ↓
SUID / Capabilities
      ↓
Scheduled Jobs
      ↓
Security Findings
```

---

## Key Takeaway

Linux security revolves around:

```text
USERS
  +
PERMISSIONS
  +
PROCESSES
  +
SERVICES
  +
FILESYSTEM
  +
NETWORKING
```

Always understand the current user's effective permissions before assessing what resources are accessible.
