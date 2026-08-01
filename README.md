# Network Setup and Security

## Overview
Designed and implemented a small office network for a fictional company, 
Golden Crunch Biscuits Ltd, using Cisco Packet Tracer. The project covers 
network design, firewall rule configuration, port scanning, vulnerability 
hardening and live traffic analysis using Wireshark.

## Objectives
- Design a simple and secure office network topology
- Configure firewall ACL rules to block insecure protocols
- Perform port scanning with Nmap to identify exposed services
- Harden the system by closing unnecessary open ports
- Capture and analyse live network traffic with Wireshark

## Tools Used
| Tool | Purpose |
|---|---|
| Cisco Packet Tracer | Network design and simulation |
| Windows Defender Firewall | Host-based firewall rule configuration |
| Nmap / Zenmap | Port scanning and service discovery |
| Wireshark | Live packet capture and traffic analysis |
| PowerShell | Rule testing and service management |

## What Was Done

### 1. Network Design
Built a small office network consisting of a router, firewall, switch, 
3 workstations, a printer, a server and an IP phone. All devices assigned 
static IPs within the 192.168.10.0/24 subnet. A firewall ACL blocked 
inbound Telnet (port 23) from all external sources while permitting all 
other traffic.

**ACL Rule Applied:**
access-list OUTSIDE-IN deny tcp any any eq 23
access-list OUTSIDE-IN permit ip any any

### 2. Firewall Rule Configuration
Used Windows Defender Firewall to create inbound rules:
- Blocked TCP port 23 (Telnet) — verified with Test-NetConnection
- Allowed TCP port 80 (HTTP) — verified with Python HTTP server

**Test Result:** TcpTestSucceeded: False confirmed port 23 successfully blocked.

### 3. Port Scanning and Hardening
Ran an Nmap intensive scan (nmap -T4 -A -v localhost) which discovered 
several open ports including port 5432 (PostgreSQL). A firewall rule was 
created to block port 5432. When the port remained open due to the running 
service, the PostgreSQL process was identified and terminated via PowerShell:

taskkill /PID 6116 /F

A follow-up Nmap scan confirmed successful remediation.

### 4. Wireshark Traffic Analysis
Captured and filtered four types of live network traffic:

| Traffic Type | Filter | Finding |
|---|---|---|
| HTTP | http | Unencrypted GET requests visible in plaintext |
| DNS | dns | Domain name resolution queries captured |
| ICMP | icmp | Ping requests and replies between devices |
| HTTPS/TLS | tls | Encrypted traffic — content unreadable |

## Key Lessons
- A firewall rule blocks traffic but does not stop the underlying service
- Nmap reveals the attack surface an attacker would see
- HTTP traffic is readable in Wireshark — TLS protects data in transit
- Telnet transmits credentials in plaintext and should never be used

## Skills Demonstrated
- Network design and topology planning
- Firewall rule configuration (Cisco ACL and Windows Defender)
- Port scanning and service enumeration with Nmap
- Attack surface reduction and port hardening
- Network traffic analysis with Wireshark
- PowerShell for security verification
