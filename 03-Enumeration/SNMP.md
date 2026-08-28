# 📡 SNMP Enumeration

> SNMP (Simple Network Management Protocol) is used to monitor and manage network-connected devices. Misconfigured SNMP can expose extensive information about hosts, network interfaces, processes, installed software, and device configuration.

---

## Overview

SNMP is commonly found on:

- Routers
- Switches
- Firewalls
- Servers
- Printers
- UPS devices
- Network appliances

Common port:

```text
161/UDP → SNMP queries
```

SNMP traps commonly use:

```text
162/UDP
```

---

## Why SNMP Matters

SNMP can expose far more information than beginners expect.

Depending on the device and configuration, enumeration may reveal:

```text
Hostname
Operating System
Network Interfaces
IP Addresses
Routes
Running Processes
Installed Software
Device Description
Contact Information
Location
```

This can make SNMP extremely valuable during internal network enumeration.

---

# SNMP Versions

The major versions are:

```text
SNMPv1
SNMPv2c
SNMPv3
```

---

## SNMPv1

Legacy implementation.

Uses community strings for access control.

Does not provide modern cryptographic protection.

---

## SNMPv2c

Improves protocol functionality but still commonly uses community strings.

The `c` stands for:

```text
Community-Based
```

---

## SNMPv3

Introduces stronger security capabilities including:

- Authentication
- Integrity
- Encryption/privacy

SNMPv3 should generally be preferred for modern deployments.

---

# Community Strings

SNMPv1/v2c commonly use a **community string** that behaves somewhat like a shared access credential.

Common historical defaults include:

```text
public
private
```

Typical convention:

```text
public  → Read-only
private → Read-write
```

Real environments should not rely on predictable/default community strings.

---

# Initial Discovery

Because SNMP primarily uses UDP, include UDP enumeration.

Authorized/lab example:

```bash
sudo nmap -sU -p 161 TARGET
```

Version/service detection:

```bash
sudo nmap -sU -sV -p 161 TARGET
```

UDP scans can be slower and less deterministic than TCP scans.

---

# Nmap SNMP Scripts

List installed scripts:

```bash
ls /usr/share/nmap/scripts/snmp*
```

Depending on your Nmap version, scripts may enumerate information such as:

- System details
- Interfaces
- Processes
- Software

Always review script behavior before execution.

---

# snmpwalk

`snmpwalk` recursively queries SNMP objects.

Authorized/lab example for SNMPv2c:

```bash
snmpwalk -v2c -c COMMUNITY TARGET
```

Example:

```bash
snmpwalk -v2c -c public 192.0.2.10
```

Only use a community string you are authorized to test.

---

# snmpget

`snmpget` requests a specific SNMP object.

Conceptually:

```bash
snmpget -v2c -c COMMUNITY TARGET OID
```

`snmpwalk` is generally more useful for broad enumeration, while `snmpget` is useful when you already know the object you want.

---

# MIB and OID

SNMP organizes information using:

```text
MIB
```

Management Information Base

and:

```text
OID
```

Object Identifier.

Think of an OID as an address identifying a particular piece of management information.

---

# OID Structure

Example:

```text
1.3.6.1.2.1...
```

Different OID branches represent different categories of information.

---

# Common Information Areas

SNMP may provide data about:

### System

```text
Hostname
Description
Uptime
Location
Contact
```

### Interfaces

```text
Interface Names
MAC Addresses
IP Addresses
Interface Status
```

### Processes

```text
Running Processes
```

### Network

```text
Routes
Connections
Interfaces
```

The exact information depends on the device's MIB implementation and permissions.

---

# Useful System OID

A commonly queried system-description OID is:

```text
1.3.6.1.2.1.1.1.0
```

Conceptually:

```bash
snmpget -v2c -c COMMUNITY TARGET 1.3.6.1.2.1.1.1.0
```

This may reveal device or operating-system information.

---

# SNMP Enumeration Workflow

```text
161/UDP
   ↓
Identify SNMP
   ↓
Determine Version
   ↓
Authorized Community / Credentials
   ↓
Walk MIB
   ↓
System Information
   ↓
Interfaces
   ↓
Processes
   ↓
Network Information
   ↓
Correlate with Other Services
```

---

# SNMP + Network Mapping

Suppose SNMP reveals:

```text
eth0 → 10.10.10.20
eth1 → 172.16.5.1
```

This may indicate the host is connected to multiple networks.

That information can become important later when understanding network segmentation and authorized pivoting scenarios.

---

# SNMP + Processes

If process information is exposed, you may discover services not immediately obvious from external port scanning.

For example:

```text
Database process
Monitoring agent
Backup software
Custom application
```

This provides additional enumeration context.

---

# SNMP + Hostnames

SNMP device descriptions may reveal:

```text
RTR-CORE-01
SW-FLOOR2
PRINT-HR-01
```

Naming conventions can help understand the network's structure.

---

# SNMPv3

SNMPv3 may require:

```text
Username
Authentication method
Authentication credential
Privacy/encryption settings
```

The exact configuration depends on the security level being used.

Unlike v1/v2c, SNMPv3 is designed to provide substantially stronger authentication and privacy controls.

---

# Red Team Perspective

SNMP can act like an information map.

A single properly authorized SNMP query might reveal:

```text
Host
 ↓
Interfaces
 ↓
Network Routes
 ↓
Processes
 ↓
Software
 ↓
Additional Infrastructure
```

This is why UDP enumeration should not be ignored.

---

# Enumeration Checklist

When you discover UDP 161:

```text
□ SNMP version
□ Authentication model
□ Community string exposure, if applicable
□ System description
□ Hostname
□ Interfaces
□ IP addresses
□ Routes
□ Processes
□ Installed software
□ Device location/contact metadata
```

---

# Example Notes

```text
Host:
10.10.10.50

Port:
161/UDP

Protocol:
SNMPv2c

Access:
Read-only community available in lab

Hostname:
RTR01

Interfaces:
10.10.10.50
172.16.1.1

Interesting:
Multi-homed device
```

---

# Defensive Considerations

Administrators should:

- Prefer SNMPv3
- Disable unused SNMP
- Restrict management access
- Avoid default community strings
- Use strong authentication
- Limit exposed OIDs/information
- Monitor SNMP traffic

---

# Common Beginner Mistakes

- Scanning only TCP and missing SNMP.
- Assuming UDP 161 will always respond clearly.
- Treating `public` as guaranteed.
- Confusing community strings with SNMPv3 authentication.
- Ignoring interface/routing information.
- Dumping data without analyzing it.
- Attempting write operations without explicit authorization.

---

# Interview Questions

### What is SNMP?

A protocol for monitoring and managing network-connected devices.

### Default SNMP port?

UDP 161.

### What is port 162?

Commonly used for SNMP traps/notifications.

### What is a community string?

An access-control string used primarily by SNMPv1/v2c.

### What is an OID?

An Object Identifier referencing a particular managed object.

### Why is SNMPv3 preferred?

It supports stronger authentication, integrity, and privacy protections.

---

# Quick Revision

```text
Ports:

161/UDP → SNMP
162/UDP → Traps

Versions:

v1
v2c
v3

Tools:

nmap
snmpwalk
snmpget

Workflow:

161
 ↓
Version
 ↓
Access
 ↓
MIB/OIDs
 ↓
System
 ↓
Interfaces
 ↓
Processes
 ↓
Network
```

---

# Practice

- TryHackMe SNMP/network-service labs
- Hack The Box
- HTB Academy
- Metasploitable
- Your own SNMP-enabled lab device

---

# References

- RFC 1157 — SNMPv1
- RFC 3411 — SNMPv3 Architecture
- Net-SNMP Documentation
- Nmap NSE Documentation
