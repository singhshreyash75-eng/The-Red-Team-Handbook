
# Docker Labs — Cybersecurity Lab Environments

> Building isolated, reproducible, container-based security laboratories using Docker and Docker Compose.

## 1. What is Docker?

**Docker** is a container platform used to package applications with their dependencies.

Containers share the host operating-system kernel while providing process and filesystem isolation.

```text
Host Operating System
        ↓
Docker Engine
        ↓
Containers
```

Docker is widely used for:

- Application development
- Testing
- CI/CD
- Security research
- Vulnerable application labs
- Reproducible environments

---

## 2. Containers vs Virtual Machines

| Containers | Virtual Machines |
|---|---|
| Share host kernel | Have guest OS kernel |
| Lightweight | More resource-intensive |
| Fast startup | Slower startup |
| Process isolation | Hardware-assisted virtualization |
| Application-focused | Full OS environments |

Important: containers are not equivalent to strong virtual-machine isolation.

---

## 3. Why Docker for Security Labs?

Docker provides:

```text
Reproducibility
Isolation
Fast Deployment
Easy Reset
Multiple Services
Version Control
```

Conceptually:

```text
Docker Image
      ↓
Container
      ↓
Vulnerable Application
      ↓
Controlled Testing
```

---

## 4. Basic Docker Commands

Check version:

```bash
docker --version
```

Engine information:

```bash
docker info
```

Running containers:

```bash
docker ps
```

All containers:

```bash
docker ps -a
```

Images:

```bash
docker images
```

---

## 5. Running a Container

Example using an official lightweight image:

```bash
docker run --rm alpine:3.20 echo "Hello Lab"
```

The container executes the command and exits.

---

## 6. Interactive Container

```bash
docker run --rm -it alpine:3.20 sh
```

Inside:

```bash
whoami
pwd
ls
```

Exit:

```bash
exit
```

---

## 7. Port Mapping

Docker publishes container ports using:

```text
Host Port : Container Port
```

Example:

```bash
docker run --rm -p 127.0.0.1:8080:80 nginx:alpine
```

Access:

```text
http://127.0.0.1:8080
```

Binding to `127.0.0.1` helps prevent external network exposure.

---

## 8. Docker Networking

Common network drivers:

```text
bridge
host
none
overlay
```

For security labs, bridge networks are frequently used.

Create a lab network:

```bash
docker network create lab-network
```

Inspect:

```bash
docker network inspect lab-network
```

---

## 9. Container Isolation

Security considerations:

```text
Capabilities
Namespaces
Cgroups
Filesystem permissions
Network access
Docker socket
```

Avoid:

```text
Privileged containers
Host network mode
Unnecessary host mounts
Docker socket exposure
```

for untrusted lab workloads.

---

## 10. Docker Compose

Docker Compose defines multi-container applications using YAML.

Example:

```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "127.0.0.1:8080:80"
```

Start:

```bash
docker compose up -d
```

Stop:

```bash
docker compose down
```

---

## 11. Multi-Container Architecture

```text
Browser
   ↓
Web Container
   ↓
Database Container
```

Docker Compose can coordinate:

```text
Web Applications
Databases
Caches
Internal Services
```

---

## 12. Lab Directory Structure

Recommended:

```text
security-labs/
├── docker-compose.yml
├── README.md
├── config/
├── data/
└── reports/
```

Keep secrets and sensitive results outside public repositories.

---

## 13. Docker Logs

View logs:

```bash
docker logs <container>
```

Follow:

```bash
docker logs -f <container>
```

Compose:

```bash
docker compose logs -f
```

Logs help diagnose failed deployments and application behavior.

---

## 14. Container Inspection

```bash
docker inspect <container>
```

Resource usage:

```bash
docker stats
```

Processes:

```bash
docker top <container>
```

---

## 15. Execute Commands

For a container you administer:

```bash
docker exec -it <container> sh
```

Useful for:

```text
Debugging
Configuration inspection
Application troubleshooting
```

---

## 16. Volumes

Volumes preserve data outside the container's writable layer.

List:

```bash
docker volume ls
```

Inspect:

```bash
docker volume inspect <volume>
```

Be careful when resetting lab volumes because data may be permanently deleted.

---

## 17. Security Lab Isolation

Recommended:

```text
Host
  |
  +-- Docker Engine
         |
         +-- Isolated Lab Network
                |
                +-- Vulnerable Web App
                +-- Test Database
```

Security practices:

- Bind web ports to localhost.
- Avoid privileged mode.
- Do not mount host secrets.
- Use dedicated lab data.
- Keep Docker updated.
- Disable labs when unused.

---

## 18. macOS Considerations

Docker Desktop on macOS runs Linux containers through a Linux virtual-machine environment.

Useful commands remain:

```bash
docker ps
docker compose up -d
docker compose down
```

On Apple Silicon, image architecture matters.

Check:

```bash
docker image inspect <image>
```

Some older x86-only lab images may require emulation or an alternative environment.

---

## 19. Lab Reset

Stop:

```bash
docker compose down
```

Rebuild:

```bash
docker compose up -d --build
```

Remove associated volumes:

```bash
docker compose down -v
```

Use `-v` only when intentionally deleting lab state.

---

## 20. Common Vulnerable Applications

Examples:

```text
DVWA
OWASP Juice Shop
WebGoat
Other purpose-built training applications
```

Deploy only official or reviewed images and configuration files.

---

## 21. Docker Security Checklist

```text
[ ] Trusted images
[ ] Pinned versions where practical
[ ] Localhost-only published ports
[ ] No privileged containers
[ ] No unnecessary host mounts
[ ] No Docker socket exposure
[ ] Resource limits where needed
[ ] Lab data separated
[ ] Logs reviewed
[ ] Containers stopped after use
```

---

## Key Takeaway

Docker enables:

```text
REPRODUCIBLE LABS
       +
FAST DEPLOYMENT
       +
CONTROLLED TESTING
```

However, container isolation is not a substitute for careful host security and network isolation.
