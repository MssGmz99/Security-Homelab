# Cyber Lab 01 — Nmap Network Scanning & Enumeration

## Overview
Performed network reconnaissance and service enumeration against a live lab environment using Nmap. Discovered active hosts, identified open ports, detected running services, fingerprinted the operating system, and ran an aggressive scan against a Windows Server 2012 R2 Domain Controller.

## Environment
- **Attacker Machine:** Kali Linux 2026.1 (192.168.1.138)
- **Target Machine:** Windows Server 2012 R2 — Lab-DC01 (192.168.1.200)
- **Network:** 192.168.1.0/24 (Bridged Adapter — Home Lab Network)
- **Tool:** Nmap 7.95

## Lab Network Map

| Machine | IP Address | Role |
|---|---|---|
| Home Router | 192.168.1.1 | Gateway |
| Windows 11 Host | 192.168.1.211 | Host Machine |
| Windows Server 2012 R2 | 192.168.1.200 | Domain Controller — Target |
| Kali Linux | 192.168.1.138 | Attacker Machine |

## Scans Performed

### Scan 1 — Ping Sweep (Host Discovery)
```bash
sudo nmap -sn 192.168.1.0/24
```
**Purpose:** Discover all live hosts on the network
**Result:** 5 hosts discovered including router, host machine, Roku TV, and Domain Controller

### Scan 2 — Service Version Detection
```bash
sudo nmap -sV 192.168.1.200
```
**Purpose:** Identify open ports and running services on the Domain Controller
**Result:** 18 open ports identified including DNS, Kerberos, LDAP, SMB, RPC, and WinRM

### Scan 3 — OS Detection
```bash
sudo nmap -O 192.168.1.200
```
**Purpose:** Fingerprint the operating system of the target
**Result:** Windows Server 2012 R2 identified with 97% confidence

### Scan 4 — Aggressive Scan
```bash
sudo nmap -A 192.168.1.200
```
**Purpose:** Full enumeration including OS detection, service versions, scripts, and traceroute
**Result:** Full Domain Controller fingerprint obtained including hostname, domain name, FQDN, and SMB security configuration

## Key Findings

### Open Ports on Domain Controller

| Port | Service | Significance |
|---|---|---|
| 53 | DNS — Simple DNS Plus | Domain name resolution |
| 88 | Kerberos | Domain authentication |
| 135 | MSRPC | Windows Remote Procedure Call |
| 139 | NetBIOS-SSN | Legacy Windows networking |
| 389 | LDAP — lab.local | Active Directory queries |
| 445 | Microsoft-DS | SMB file sharing |
| 464 | kpasswd5 | Kerberos password changes |
| 636 | LDAPS | Secure LDAP |
| 3268 | Global Catalog | AD Global Catalog |
| 5985 | WinRM | Remote management |

### System Information Gathered
- **OS:** Windows Server 2012 R2 Standard Evaluation 9600
- **Computer Name:** Lab-DC01
- **Domain:** lab.local
- **Forest:** lab.local
- **FQDN:** Lab-DC01.lab.local
- **SMB Signing:** Enabled and Required
- **Authentication Level:** User

## Security Observations
- SMB message signing is enabled and required — good security practice, prevents relay attacks
- WinRM port 5985 is open — remote management is accessible on the network
- Kerberos port 88 confirms this is an Active Directory Domain Controller
- LDAP port 389 is open without SSL — encrypted LDAPS on 636 is also available
- Multiple high dynamic RPC ports open — normal for Windows Server but increases attack surface

## Key Concepts Learned
- **Ping sweep (-sn)** discovers live hosts without port scanning
- **Service detection (-sV)** identifies what software is running on open ports
- **OS detection (-O)** fingerprints the target operating system
- **Aggressive scan (-A)** combines all scan types for maximum information gathering
- **SMB signing** prevents man-in-the-middle attacks on file sharing
- **FQDN** is the full domain name used to identify a host on a network
- Open ports on a DC are expected — Kerberos, LDAP, DNS, and RPC are all required for AD to function

## Defensive Takeaway
From a T1 IT perspective this scan shows why network visibility matters. Every open port is a potential entry point if not properly secured. Regular port scanning of your own infrastructure helps identify unexpected services running on servers that should not be there.

## Screenshots
See screenshots folder for documented evidence of each scan.
