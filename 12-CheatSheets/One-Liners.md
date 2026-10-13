
# One-Liners — Cheat Sheet

> Practical Bash, Linux, networking, Python, PowerShell, and security-analysis one-liners for authorized environments.

## 1. System Information

Current user:

```bash
whoami
```

Identity and groups:

```bash
id
```

OS:

```bash
cat /etc/os-release
```

Kernel:

```bash
uname -a
```

Hostname:

```bash
hostname
```

---

## 2. File Discovery

Find configuration files:

```bash
find /etc -type f -name "*.conf" 2>/dev/null
```

Find shell scripts:

```bash
find . -type f -name "*.sh"
```

Recently modified files:

```bash
find /var/log -type f -mtime -1
```

Large files:

```bash
find /home -type f -size +100M
```

---

## 3. Text Search

Find errors:

```bash
grep -i "error" app.log
```

Find matching files:

```bash
grep -Rl "TODO" ./src
```

Search multiple patterns:

```bash
grep -Ei "error|warning|failed" app.log
```

Count matching lines:

```bash
grep -ic "error" app.log
```

---

## 4. Text Processing

Deduplicate:

```bash
sort -u input.txt > unique.txt
```

Count lines:

```bash
wc -l input.txt
```

First field:

```bash
awk '{print $1}' input.txt
```

Extract CSV first field (simple CSV only):

```bash
cut -d ',' -f 1 input.csv
```

Replace text:

```bash
sed 's/old/new/g' input.txt
```

---

## 5. IP Address

Linux:

```bash
ip -br addr
```

macOS:

```bash
ifconfig
```

Public IP through an external service:

```bash
curl -s https://api.ipify.org
```

Note: this sends a request to a third-party service.

---

## 6. Network Connections

Linux listening ports:

```bash
ss -tuln
```

Linux processes:

```bash
sudo ss -tulpn
```

macOS listening TCP sockets:

```bash
lsof -nP -iTCP -sTCP:LISTEN
```

Established connections on Linux:

```bash
ss -tn state established
```

---

## 7. DNS

A record:

```bash
dig +short example.com A
```

MX records:

```bash
dig +short example.com MX
```

Nameservers:

```bash
dig +short example.com NS
```

TXT:

```bash
dig +short example.com TXT
```

Reverse lookup:

```bash
dig -x 192.0.2.10
```

---

## 8. HTTP

Headers:

```bash
curl -I https://example.com
```

Status code:

```bash
curl -s -o /dev/null -w "%{http_code}\n" https://example.com
```

Follow redirects:

```bash
curl -L -I https://example.com
```

Download:

```bash
curl -L -o page.html https://example.com
```

---

## 9. TLS

Inspect certificate:

```bash
openssl s_client -connect example.com:443 -servername example.com
```

Show local certificate information:

```bash
openssl x509 -in cert.pem -noout -text
```

Certificate expiry:

```bash
openssl x509 -in cert.pem -noout -enddate
```

---

## 10. File Hashing

Linux:

```bash
sha256sum file.txt
```

macOS:

```bash
shasum -a 256 file.txt
```

Windows PowerShell:

```powershell
Get-FileHash file.txt -Algorithm SHA256
```

---

## 11. Base64

Linux encode:

```bash
printf "hello" | base64
```

Linux decode:

```bash
printf "aGVsbG8=" | base64 -d
```

macOS decode:

```bash
printf "aGVsbG8=" | base64 -D
```

Remember:

```text
Base64 != Encryption
```

---

## 12. Processes

Linux:

```bash
ps aux
```

Find process:

```bash
pgrep -a nginx
```

Top CPU consumers:

```bash
ps aux --sort=-%cpu | head
```

macOS alternative:

```bash
ps -Ao pid,comm,%cpu | sort -k3 -nr | head
```

---

## 13. Services

Systemd running services:

```bash
systemctl list-units --type=service --state=running
```

Specific service:

```bash
systemctl status ssh
```

Recent service logs:

```bash
journalctl -u ssh --since "1 hour ago"
```

---

## 14. Permissions

Current identity:

```bash
id
```

Sudo permissions:

```bash
sudo -l
```

SUID files:

```bash
find /usr /bin /sbin -type f -perm -4000 2>/dev/null
```

File capabilities:

```bash
getcap -r /usr/bin /usr/local/bin 2>/dev/null
```

---

## 15. Cron

Current user's cron jobs:

```bash
crontab -l
```

System cron:

```bash
cat /etc/crontab
```

Scheduled directories:

```bash
ls -la /etc/cron.*
```

---

## 16. Subdomain Discovery

For an authorized domain:

```bash
subfinder -d example.com -silent
```

Amass passive enumeration:

```bash
amass enum -passive -d example.com
```

Assetfinder:

```bash
assetfinder --subs-only example.com
```

---

## 17. Live HTTP Hosts

ProjectDiscovery httpx:

```bash
httpx -l subdomains.txt -silent
```

Save:

```bash
httpx -l subdomains.txt -silent -o live.txt
```

---

## 18. Historical URLs

GAU:

```bash
gau example.com
```

Waybackurls:

```bash
echo example.com | waybackurls
```

Deduplicate:

```bash
cat gau.txt wayback.txt | sort -u > urls.txt
```

---

## 19. Crawling

Katana:

```bash
katana -u https://example.test
```

Save:

```bash
katana -u https://example.test -o katana.txt
```

---

## 20. Content Discovery

FFUF:

```bash
ffuf -w paths.txt -u https://example.test/FUZZ
```

Gobuster:

```bash
gobuster dir -u https://example.test -w paths.txt
```

Feroxbuster:

```bash
feroxbuster -u https://example.test -w paths.txt
```

Use controlled request rates and authorized targets.

---

## 21. Nmap

Basic scan:

```bash
nmap 192.0.2.10
```

Service detection:

```bash
nmap -sV 192.0.2.10
```

Selected ports:

```bash
nmap -p 22,80,443 192.0.2.10
```

Host discovery:

```bash
nmap -sn 192.0.2.0/28
```

---

## 22. Nuclei

Run reviewed templates against an authorized test host:

```bash
nuclei -u https://example.test
```

Use relevant template selection and rate limits.

A scanner match requires manual validation.

---

## 23. Packet Capture

List interfaces:

```bash
tcpdump -D
```

Capture DNS:

```bash
sudo tcpdump -nn port 53
```

Save capture:

```bash
sudo tcpdump -i en0 -w capture.pcap
```

Read capture:

```bash
tcpdump -nn -r capture.pcap
```

---

## 24. Python One-Liners

Version:

```bash
python3 --version
```

JSON pretty print:

```bash
python3 -m json.tool data.json
```

Local HTTP server:

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

Generate random token:

```bash
python3 -c 'import secrets; print(secrets.token_hex(16))'
```

Calculate SHA256:

```bash
python3 -c 'import hashlib; print(hashlib.sha256(b"hello").hexdigest())'
```

---

## 25. PowerShell One-Liners

Current user:

```powershell
whoami
```

Processes:

```powershell
Get-Process
```

Services:

```powershell
Get-Service
```

IP addresses:

```powershell
Get-NetIPAddress
```

TCP connections:

```powershell
Get-NetTCPConnection
```

Connectivity:

```powershell
Test-NetConnection example.com -Port 443
```

File hash:

```powershell
Get-FileHash file.txt -Algorithm SHA256
```

---

## 26. JSON and CSV

Pretty-print JSON with jq:

```bash
jq . data.json
```

Extract JSON field:

```bash
jq -r '.name' data.json
```

Count JSON array elements:

```bash
jq 'length' data.json
```

PowerShell CSV import:

```powershell
Import-Csv data.csv
```

---

## 27. Archives

Create tar.gz:

```bash
tar -czf archive.tar.gz folder/
```

Extract:

```bash
tar -xzf archive.tar.gz
```

Create ZIP:

```bash
zip -r archive.zip folder/
```

Extract:

```bash
unzip archive.zip
```

---

## 28. File Transfer

SCP upload:

```bash
scp file.txt user@192.0.2.10:/tmp/
```

SCP download:

```bash
scp user@192.0.2.10:/tmp/file.txt .
```

SFTP:

```bash
sftp user@192.0.2.10
```

rsync:

```bash
rsync -av folder/ user@192.0.2.10:/tmp/folder/
```

---

## 29. Log Analysis

Search errors:

```bash
grep -i "error" app.log
```

Recent systemd events:

```bash
journalctl --since "30 minutes ago"
```

Follow file:

```bash
tail -f app.log
```

Count unique log values:

```bash
sort events.txt | uniq -c | sort -nr
```

---

## 30. Useful Bash Patterns

Loop over files:

```bash
for f in *.txt; do echo "$f"; done
```

Read lines:

```bash
while IFS= read -r line; do echo "$line"; done < input.txt
```

Check file:

```bash
test -f input.txt && echo "Exists"
```

Count files:

```bash
find . -type f | wc -l
```

---

## 31. Security Assessment Workflow

```text
Scope
  ↓
Recon
  ↓
Service Discovery
  ↓
Content Discovery
  ↓
Security Checks
  ↓
Manual Validation
  ↓
Evidence
  ↓
Reporting
```

---

## Key Takeaway

One-liners are useful for:

```text
QUICK INSPECTION
      +
AUTOMATION
      +
DATA PROCESSING
      +
TROUBLESHOOTING
```

But understand each command before running it, especially when it changes files, accesses credentials, or generates network traffic.
