# 🌐 Networking Fundamentals

> Networking is the backbone of offensive security. Before attacking a target, you must understand how devices communicate, how data flows across networks, and where attackers can intercept, manipulate, or exploit that communication.

---

# Overview

Networking is the process of connecting devices so they can exchange information.

Every web application, API, server, Active Directory environment, cloud instance, and CTF machine communicates using networking protocols.

Without networking knowledge, penetration testing becomes guesswork.

---

# Learning Objectives

After reading this chapter, you should be able to:

- Understand how computer networks work
- Differentiate between LAN, WAN and Internet
- Understand IP addressing
- Understand routing
- Understand switches and routers
- Understand MAC addresses
- Understand common protocols
- Understand packet flow
- Prepare for HTTP, DNS, SMB and Active Directory

---

# What is a Network?

A network is a collection of devices connected together for communication.

Example

```text
Laptop
     │
     │
Router
     │
──────────────
│      │      │
PC    Server  Phone
```

Every device exchanges data using protocols.

---

# Types of Networks

## LAN (Local Area Network)

Small network inside:

- Home
- Office
- College

Example

```text
192.168.1.0/24
```

---

## WAN (Wide Area Network)

Connects multiple LANs.

Example

Internet.

---

## MAN

Metropolitan Area Network.

Covers an entire city.

---

## PAN

Personal Area Network.

Bluetooth

Hotspot

USB Tethering

---

# Network Devices

## Router

Responsible for forwarding packets between different networks.

Example

```text
Laptop

↓

Router

↓

Internet
```

---

## Switch

Connects devices inside a LAN.

Unlike hubs, switches forward traffic only to the intended device.

---

## Hub

Broadcasts packets to every connected device.

Rarely used today.

---

## Firewall

Filters incoming and outgoing traffic.

Can allow or block packets based on rules.

Examples

- Windows Firewall
- pfSense
- Cisco ASA

---

# IP Address

Every device connected to a network has an IP Address.

Example

```text
192.168.1.10
```

Think of it as the house address of a device.

---

## IPv4

32-bit address.

Example

```text
192.168.1.15
```

Maximum

```text
255.255.255.255
```

---

## IPv6

128-bit address.

Example

```text
2409:40f2:1000:....
```

---

# Private IP Ranges

| Range | CIDR |
|---------|------|
|10.0.0.0|/8|
|172.16.0.0|/12|
|192.168.0.0|/16|

These are not routable on the Internet.

---

# Public IP

Assigned by an ISP.

Visible on the Internet.

Find yours

```bash
curl ifconfig.me
```

---

# MAC Address

Layer 2 hardware address.

Example

```text
00:0C:29:4F:8E:2B
```

Unlike IPs, MAC addresses identify network interfaces.

View MAC

```bash
ip link
```

---

# Default Gateway

The device that forwards packets outside your local network.

Check

```bash
ip route
```

Example

```text
default via 192.168.1.1
```

---

# DNS

Instead of remembering

```text
142.250.xxx.xxx
```

we use

```text
google.com
```

DNS translates names into IP addresses.

We'll study DNS in detail later.

---

# Ports

Ports identify applications running on a host.

Example

```text
80 → HTTP

443 → HTTPS

22 → SSH

53 → DNS
```

---

# Packet

A packet is the smallest unit of data transmitted across a network.

Example

```text
Laptop

↓

Packet

↓

Router

↓

Server
```

---

# Packet Flow

Example

```text
Browser

↓

DNS Lookup

↓

TCP Connection

↓

HTTP Request

↓

HTTP Response

↓

Browser Displays Page
```

Understanding this flow is extremely important for web security.

---

# Routing

Routers decide where packets should go.

View routing table

```bash
ip route
```

Example

```text
Destination

↓

Gateway

↓

Interface
```

---

# NAT (Network Address Translation)

Allows multiple private IPs to share one public IP.

Example

```text
Laptop

192.168.1.5

↓

Router

↓

Public IP

↓

Internet
```

Almost every home router performs NAT.

---

# CIDR Notation

Example

```text
192.168.1.0/24
```

Meaning

- Network: 192.168.1.0
- Hosts: 254

Common CIDRs

| CIDR | Hosts |
|-------|-------:|
|/30|2|
|/29|6|
|/28|14|
|/27|30|
|/26|62|
|/25|126|
|/24|254|

---

# Basic Networking Commands

Current IP

```bash
ip a
```

Routing

```bash
ip route
```

Ping

```bash
ping google.com
```

Traceroute

```bash
traceroute google.com
```

DNS Lookup

```bash
nslookup google.com
```

Check Open Ports

```bash
ss -tuln
```

Network Connections

```bash
netstat -tulnp
```

---

# Red Team Perspective

Networking knowledge helps you:

- Enumerate hosts
- Discover live machines
- Understand pivoting
- Perform internal reconnaissance
- Identify exposed services
- Scan networks with Nmap
- Understand Active Directory communication
- Analyze packet captures in Wireshark

Every Red Team assessment begins with networking.

---

# Common Beginner Mistakes

- Confusing IP and MAC addresses
- Ignoring private IP ranges
- Forgetting the role of DNS
- Memorizing ports without understanding protocols
- Not checking routing tables

---

# Interview Questions

### What is a network?

### Difference between Router and Switch?

### Difference between Public and Private IP?

### What is NAT?

### What is a MAC Address?

### Difference between IPv4 and IPv6?

### What is a packet?

### What is a port?

---

# Quick Revision

- Network = Connected devices
- Router connects networks
- Switch connects devices in a LAN
- IP identifies a host
- MAC identifies a network interface
- DNS resolves names into IPs
- NAT allows private devices to share one public IP
- Ports identify applications
- Packets carry data
- Routing decides packet paths

---

# Practice

TryHackMe

- Intro to Networking
- Network Fundamentals

Hack The Box Academy

- Introduction to Networking

Cisco Skills for All

- Networking Basics

---

# References

- Cisco Networking Academy
- RFC 791 (IPv4)
- RFC 2460 (IPv6)
- Linux Networking Documentation
- TryHackMe
- Hack The Box Academy
