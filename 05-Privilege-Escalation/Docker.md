# Docker

> Docker fundamentals and Linux security enumeration notes.

## What is Docker?

Docker runs applications inside **containers**.

Containers isolate processes while normally sharing the host's Linux kernel.

Important terms:

```text
Image       -> template used to create containers
Container   -> running instance of an image
Dockerfile  -> instructions for building an image
Registry    -> repository containing images
Volume      -> persistent/shared storage
Bind mount  -> host path mounted inside a container
```

## Basic Enumeration

Check Docker:

```bash
docker --version
```

List containers:

```bash
docker ps
docker ps -a
```

List images:

```bash
docker images
```

Inspect container:

```bash
docker inspect <container>
```

View logs:

```bash
docker logs <container>
```

## Shell Inside a Container

```bash
docker exec -it <container> /bin/bash
```

If Bash is unavailable:

```bash
docker exec -it <container> /bin/sh
```

## Docker Group

Check current user/group membership:

```bash
id
groups
```

Look for:

```text
docker
```

Membership in the `docker` group is security-sensitive because it normally grants extensive control over the Docker daemon.

## Docker Socket

Common socket:

```text
/var/run/docker.sock
```

Inspect:

```bash
ls -l /var/run/docker.sock
```

Access to a privileged Docker daemon can provide substantial control over containers and host resources.

## Mounts

Inspect:

```bash
docker inspect <container>
```

Look for:

- Host bind mounts
- Sensitive directories
- Writable host paths
- Docker socket exposure
- Secrets/configuration mounted into containers

## Privileged Containers

Security-relevant Docker settings include:

```text
Privileged
CapAdd
SecurityOpt
Mounts
User
```

A privileged container has significantly weaker isolation from its host.

## Dockerfile Basics

Example:

```dockerfile
FROM ubuntu:24.04

WORKDIR /app

COPY . /app

CMD ["./app"]
```

Common instructions:

```text
FROM       base image
RUN        execute build-time command
COPY       copy files into image
WORKDIR    set working directory
ENV        define environment variable
EXPOSE     document application port
USER       select runtime user
CMD        default command
ENTRYPOINT primary executable
```

## Security Checklist

Check for:

- Containers running as root
- `--privileged`
- Excessive Linux capabilities
- Exposed Docker socket
- Sensitive host bind mounts
- Embedded credentials/secrets
- Writable sensitive paths
- Outdated/untrusted images

## Quick Enumeration

```bash
id
groups
docker ps -a
docker images
ls -l /var/run/docker.sock
```

## Key Takeaway

A container is **not automatically a strong security boundary**.

Docker security depends heavily on:

```text
daemon access
+ mounts
+ capabilities
+ runtime user
+ container configuration
```
