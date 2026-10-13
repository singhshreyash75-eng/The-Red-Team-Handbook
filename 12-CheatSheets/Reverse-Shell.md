
# Reverse Shell — Cheat Sheet

> Reverse-shell fundamentals, network behavior, controlled lab demonstrations, detection, and defensive analysis.

## 1. What is a Reverse Shell?

A **reverse shell** is a command shell whose communication connection is initiated from the system running the shell toward another system.

Conceptually:

```text
Lab Target
    |
Outbound Connection
    |
    v
Lab Controller
```

The connection direction is the defining feature.

---

## 2. Reverse vs Bind Shell

### Reverse Shell

```text
Target
  |
  | Initiates connection
  v
Controller
```

### Bind Shell

```text
Controller
    |
    | Initiates connection
    v
Target Listener
```

Comparison:

| Reverse Shell | Bind Shell |
|---|---|
| Target initiates connection | Controller initiates connection |
| Requires outbound connectivity | Requires inbound connectivity |
| Connection direction is reversed | Target accepts connections |
| Can be restricted by egress filtering | Can be restricted by ingress filtering |

---

## 3. Important Components

```text
Controller
Target
Network connection
Shell process
Input/output streams
```

A network shell typically connects process input and output to a network transport.

---

## 4. Shell Types

Common command interpreters:

```text
/bin/sh
/bin/bash
cmd.exe
powershell.exe
```

Different shells have different capabilities and syntax.

---

## 5. TTY vs PTY

**TTY** is a terminal interface.

**PTY** is a pseudo-terminal.

Interactive programs may expect terminal functionality such as:

```text
Line editing
Terminal dimensions
Signals
Job control
```

A basic network connection does not automatically provide a fully functional terminal.

---

## 6. Controlled Connectivity Demonstration

Before examining shell behavior, verify ordinary TCP connectivity in an isolated lab.

Controller terminal:

```bash
nc -l 127.0.0.1 9001
```

Second terminal:

```bash
printf "lab-connectivity-test\n" | nc 127.0.0.1 9001
```

Expected result:

```text
lab-connectivity-test
```

This demonstrates a local TCP connection without providing remote command execution.

Netcat listener syntax may vary by implementation.

---

## 7. Network Flow

```text
Application / Process
        ↓
TCP Connection
        ↓
Network Interface
        ↓
Receiving Host
```

Relevant observations:

```text
Source IP
Destination IP
Source port
Destination port
Process identity
Connection duration
```

---

## 8. Network Diagnostics

Linux connections:

```bash
ss -tnp
```

macOS:

```bash
lsof -nP -iTCP
```

Windows PowerShell:

```powershell
Get-NetTCPConnection
```

These commands help identify established TCP connections.

---

## 9. Packet Capture

Capture controlled lab traffic:

```bash
sudo tcpdump -nn -i lo port 9001
```

On macOS, loopback is typically:

```text
lo0
```

Example:

```bash
sudo tcpdump -nn -i lo0 port 9001
```

---

## 10. Wireshark Analysis

Useful display filter:

```text
tcp.port == 9001
```

Inspect:

```text
TCP handshake
Connection timing
Packet sizes
Connection termination
```

For plaintext lab traffic, Follow TCP Stream can reconstruct the conversation.

---

## 11. Why Reverse Connections Matter

Some environments permit outbound connections more readily than inbound connections.

Conceptually:

```text
Inbound Firewall
      |
Restricted
```

```text
Outbound Firewall
      |
Policy-dependent
```

This is why outbound connection control matters.

A reverse connection is not inherently malicious; intent and process behavior determine risk.

---

## 12. Egress Filtering

**Egress filtering** restricts outbound network communication.

Examples:

```text
Allowed destination networks
Approved proxy usage
Permitted ports
Application-aware firewall policies
DNS restrictions
```

Strong egress controls can limit unauthorized outbound channels.

---

## 13. Process Relationships

A suspicious pattern may involve:

```text
Web Server Process
        ↓
Unexpected Shell Process
        ↓
External Network Connection
```

Defenders should investigate unusual parent-child process relationships.

Examples of useful telemetry:

```text
Process creation
Command line
Parent process
Network destination
User identity
```

---

## 14. Detection

Potential indicators:

```text
Unexpected outbound TCP connection
Shell process with unusual parent
Command interpreter connected to network
Unrecognized destination
Long-lived interactive traffic
```

None of these individually proves malicious activity.

---

## 15. Linux Investigation

Inspect processes:

```bash
ps auxf
```

Connections:

```bash
ss -tnp
```

Executable path:

```bash
readlink -f /proc/<PID>/exe
```

Process command line:

```bash
tr '\0' ' ' < /proc/<PID>/cmdline
```

Access to process details depends on permissions.

---

## 16. Windows Investigation

Processes:

```powershell
Get-Process
```

Network connections:

```powershell
Get-NetTCPConnection
```

Process metadata:

```powershell
Get-CimInstance Win32_Process |
Select-Object ProcessId, ParentProcessId, Name, CommandLine
```

Correlate process creation and network events.

---

## 17. Relevant Security Telemetry

Windows environments may use:

```text
Security Event 4688
Sysmon Event 1
Sysmon Event 3
```

These can provide process and network-connection information when configured.

Linux environments may use:

```text
auditd
journald
EDR telemetry
Process accounting
Network monitoring
```

---

## 18. Defensive Controls

```text
Egress filtering
Least privilege
Application allowlisting
EDR
Process monitoring
Network segmentation
Restricted shell execution
Patch management
```

---

## 19. Authorized Lab Workflow

```text
Isolated Network
      ↓
Verify Connectivity
      ↓
Observe TCP Session
      ↓
Inspect Process Behavior
      ↓
Capture PCAP
      ↓
Analyze Logs
      ↓
Test Defensive Controls
```

Use a dedicated lab with explicit authorization for any actual remote-shell exercises.

---

## Key Takeaway

The essential distinction:

```text
REVERSE
=
Target initiates connection
```

```text
BIND
=
Target accepts connection
```

For defenders, the most important relationship is:

```text
PROCESS
   +
NETWORK CONNECTION
   +
USER CONTEXT
   +
DESTINATION
```

A network connection becomes security-relevant when its process behavior and authorization context indicate a trust-boundary violation.
