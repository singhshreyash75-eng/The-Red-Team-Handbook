
# File Transfer — Cheat Sheet

> Practical Linux, Windows, macOS, SSH, HTTP, and network file-transfer methods for authorized system administration and security labs.

## 1. Overview

File transfer is frequently required for:

```text
Moving scripts
Collecting logs
Transferring PCAP files
Sharing test data
Moving reports
Copying configuration
Lab automation
```

Common methods:

```text
SCP
SFTP
rsync
curl
wget
Python HTTP Server
PowerShell
SMB
```

Prefer encrypted, authenticated transfer methods.

---

## 2. SCP

**SCP = Secure Copy**

Uses SSH for encrypted file transfer.

Local to remote:

```bash
scp file.txt user@192.0.2.10:/tmp/
```

Remote to local:

```bash
scp user@192.0.2.10:/tmp/file.txt .
```

Directory:

```bash
scp -r myfolder user@192.0.2.10:/tmp/
```

Custom SSH port:

```bash
scp -P 2222 file.txt user@192.0.2.10:/tmp/
```

---

## 3. SFTP

**SFTP = SSH File Transfer Protocol**

Connect:

```bash
sftp user@192.0.2.10
```

Useful commands:

```text
ls
pwd
lpwd
cd
lcd
get
put
mkdir
bye
```

Download:

```text
get report.txt
```

Upload:

```text
put script.sh
```

SFTP provides authenticated encrypted transfer.

---

## 4. rsync

Efficient synchronization:

```bash
rsync -av folder/ user@192.0.2.10:/tmp/folder/
```

Download:

```bash
rsync -av user@192.0.2.10:/var/log/app/ ./logs/
```

Useful options:

```text
-a    archive mode
-v    verbose
-z    compression
-n    dry run
```

Dry run:

```bash
rsync -avn folder/ user@192.0.2.10:/tmp/folder/
```

Always check trailing-slash semantics.

---

## 5. curl Download

Download file:

```bash
curl -o file.txt https://example.com/file.txt
```

Preserve remote filename:

```bash
curl -O https://example.com/file.txt
```

Follow redirects:

```bash
curl -L -o file.txt https://example.com/file.txt
```

---

## 6. curl Upload

For an authorized endpoint supporting HTTP PUT:

```bash
curl -T file.txt https://upload.example.test/file.txt
```

Multipart upload:

```bash
curl -F "file=@report.txt" https://upload.example.test/upload
```

Exact syntax depends on the server API.

Use HTTPS for sensitive files.

---

## 7. wget

Download:

```bash
wget https://example.com/file.txt
```

Custom filename:

```bash
wget -O output.txt https://example.com/file.txt
```

Continue interrupted transfer:

```bash
wget -c https://example.com/large-file.zip
```

---

## 8. Python HTTP Server

Start a basic HTTP server in the current directory:

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

Access:

```text
http://127.0.0.1:8000/
```

This is suitable for controlled local lab sharing.

Important:

```text
Python http.server
=
Basic HTTP server
```

It is not a production-grade secure file-sharing service.

Do not expose directories containing sensitive files.

---

## 9. HTTP Server on Lab Network

For an isolated, trusted lab network, a server may bind to a specific private interface.

Conceptually:

```text
Machine A
   |
HTTP Server
   |
   v
Machine B
```

Use network controls and only serve intentionally shared files.

HTTP itself does not encrypt transferred content.

---

## 10. PowerShell Download

On Windows:

```powershell
Invoke-WebRequest `
-Uri "https://example.com/file.txt" `
-OutFile "file.txt"
```

Alternative:

```powershell
Invoke-RestMethod -Uri "https://example.com/api"
```

The latter is particularly useful for structured API responses.

---

## 11. PowerShell Upload

For an authorized API accepting multipart form data:

```powershell
Invoke-RestMethod `
-Uri "https://upload.example.test/upload" `
-Method Post `
-Form @{ file = Get-Item "report.txt" }
```

Support depends on PowerShell version and endpoint requirements.

---

## 12. SMB File Sharing

**SMB = Server Message Block**

Commonly used for Windows file sharing.

UNC path:

```text
\\server\share
```

Windows:

```cmd
net use Z: \\server\share
```

PowerShell:

```powershell
Get-SmbShare
```

On Linux, authorized SMB access can be performed with tools such as:

```text
smbclient
```

SMB authentication and permissions must be configured correctly.

---

## 13. SMB with smbclient

List accessible shares using approved credentials:

```bash
smbclient -L //192.0.2.10 -U alice
```

Connect:

```bash
smbclient //192.0.2.10/shared -U alice
```

Interactive commands:

```text
ls
get
put
cd
exit
```

---

## 14. netcat

Netcat can move raw bytes between systems in controlled labs, but it provides no built-in encryption or authentication.

Prefer:

```text
SCP
SFTP
rsync over SSH
HTTPS
```

for real administrative transfers.

Raw network transfers can expose sensitive data and should not be used over untrusted networks.

---

## 15. Base64 Transfer

Base64 can represent binary data as text.

Encode:

```bash
base64 file.bin > file.b64
```

Decode on Linux:

```bash
base64 -d file.b64 > file.bin
```

macOS supports decoding with:

```bash
base64 -D -i file.b64 -o file.bin
```

Important:

```text
Base64 != Encryption
```

It does not protect confidentiality.

---

## 16. File Integrity

Verify transferred files with a cryptographic hash.

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

Compare:

```text
Original SHA256
      =
Transferred SHA256
```

Matching hashes provide strong evidence of file integrity.

---

## 17. Archive Files

Create tar archive:

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

Extract:

```bash
unzip archive.zip
```

---

## 18. SSH Key Authentication

Generate a key:

```bash
ssh-keygen -t ed25519
```

Use a key:

```bash
scp -i ~/.ssh/id_ed25519 file.txt user@192.0.2.10:/tmp/
```

Protect private keys and verify server host keys.

---

## 19. File Permissions

After transfer:

```bash
ls -l file.txt
```

Restrict sensitive files:

```bash
chmod 600 sensitive.txt
```

Make a trusted script executable:

```bash
chmod +x script.sh
```

Do not execute transferred scripts without reviewing their source and provenance.

---

## 20. Transfer Security Checklist

```text
[ ] Source and destination authorized
[ ] Encrypted transport
[ ] Server identity verified
[ ] Correct destination path
[ ] No unnecessary public exposure
[ ] Sensitive files protected
[ ] SHA256 verified where appropriate
[ ] Temporary files cleaned up
```

---

## 21. Tool Selection

| Requirement | Preferred Method |
|---|---|
| Secure file copy | SCP / SFTP |
| Directory synchronization | rsync over SSH |
| HTTP download | curl / wget |
| API upload | curl / PowerShell |
| Windows file sharing | SMB |
| Quick local lab sharing | Python HTTP server |
| Integrity verification | SHA256 |

---

## Key Takeaway

File transfer security depends on:

```text
AUTHENTICATION
     +
ENCRYPTION
     +
AUTHORIZATION
     +
INTEGRITY
```

Prefer secure protocols and avoid exposing sensitive files through unauthenticated temporary servers.
