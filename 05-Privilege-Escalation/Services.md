# Services

> Linux services enumeration and security notes.

## What is a Service?

A **service** is a program that usually runs in the background and provides functionality such as SSH, HTTP, databases, scheduled jobs, or application services.

Modern Linux systems commonly manage services with **systemd**.

## Basic Enumeration

```bash
systemctl --type=service
systemctl --type=service --state=running
systemctl list-unit-files --type=service
```

Inspect a specific service:

```bash
systemctl status <service>
systemctl cat <service>
systemctl show <service>
```

Older systems may also use:

```bash
service --status-all
```

## Processes and Listening Services

```bash
ps aux
ps -ef
ss -tulpn
```

Useful combination:

```bash
ps aux | grep <name>
```

## Common Service File Locations

```text
/etc/systemd/system/
/lib/systemd/system/
/usr/lib/systemd/system/
```

SysV-style systems may use:

```text
/etc/init.d/
```

## Important systemd Fields

Example:

```ini
[Service]
User=root
ExecStart=/opt/app/start.sh
```

Important fields to inspect:

- `User=` — account running the service
- `Group=` — service group
- `ExecStart=` — executable or script launched
- `ExecStartPre=` / `ExecStartPost=` — additional commands
- `EnvironmentFile=` — external environment/configuration file
- `WorkingDirectory=` — working directory

## Permission Checks

If a privileged service executes a file, inspect its permissions:

```bash
ls -l /opt/app/start.sh
ls -ld /opt/app
namei -l /opt/app/start.sh
```

Check:

- Does the service run as `root`?
- Can an unprivileged user modify its executable/script?
- Is a parent directory writable?
- Are configuration/environment files writable?
- Does the service reference files through unsafe paths?

## Logs

```bash
journalctl -u <service>
journalctl -u <service> --since today
```

## Quick Enumeration

```bash
systemctl --type=service --state=running
ps aux
ss -tulpn
```

## Key Takeaway

A service becomes security-sensitive when a **privileged execution context** depends on files, scripts, configuration, or directories that a lower-privileged user can modify.
