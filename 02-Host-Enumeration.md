# 02 - Host Enumeration

## Objective

The goal of the host enumeration phase is to identify:

- Operating Systems
- Hostnames
- Open Ports
- Running Services
- Network Roles
- Potential High-Value Targets

This phase transforms a list of IP addresses into a categorized inventory of systems and their functions.

---

## Step 1 - Identify Hostnames

Perform reverse DNS lookups whenever possible.

```bash
host 10.10.10.10
```

Example:

```text
10.10.10.10.in-addr.arpa domain name pointer dc01.company.local.
```

Bulk resolution:

```bash
for ip in $(cat hosts.txt); do
    host $ip
done
```

### Reporting Notes

```text
10.10.10.10    dc01.company.local
10.10.10.11    dc02.company.local
10.10.10.20    file01.company.local
```

---

## Step 2 - Identify Live Hosts

Validate previously discovered hosts.

```bash
fping -a -f hosts.txt
```

### Why

- Eliminate inactive hosts
- Validate network accessibility
- Improve scan efficiency

---

## Step 3 - Identify Open Ports

Quick TCP scan.

```bash
nmap -Pn 10.10.10.10
```

Example:

```text
53/tcp
88/tcp
135/tcp
389/tcp
445/tcp
```

Comprehensive scan:

```bash
nmap -Pn -p- 10.10.10.10
```

### What to Look For

Common services:

```text
22     SSH
80     HTTP
443    HTTPS
445    SMB
389    LDAP
88     Kerberos
3389   RDP
1433   MSSQL
5985   WinRM
```

---

## Step 4 - Identify Service Versions

Determine service versions.

```bash
nmap -Pn -sV 10.10.10.10
```

Example:

```text
445/tcp  microsoft-ds
389/tcp  Microsoft Windows Active Directory LDAP
88/tcp   Microsoft Kerberos
```

---

## Step 5 - Identify Operating Systems

Perform operating system detection.

```bash
nmap -Pn -O 10.10.10.10
```

Example:

```text
Microsoft Windows Server 2022
```

Alternative:

```bash
nmap -A 10.10.10.10
```

Potential findings:

```text
Windows Server
Ubuntu Linux
CentOS
Debian
VMware Appliance
```

---

## Step 6 - Identify SMB Hosts

Locate SMB-enabled systems.

```bash
nmap -Pn -p445 10.10.10.0/24
```

Example:

```text
10.10.10.10
10.10.10.11
10.10.10.20
10.10.10.50
```

These systems often include:

- Domain Controllers
- File Servers
- Workstations

---

## Step 7 - Identify Administrative Services

Search for remote management services.

### SSH

```bash
nmap -Pn -p22 10.10.10.0/24
```

### RDP

```bash
nmap -Pn -p3389 10.10.10.0/24
```

### WinRM

```bash
nmap -Pn -p5985,5986 10.10.10.0/24
```

Potential findings:

```text
Linux Servers
Windows Servers
Jump Hosts
Administration Servers
```

---

## Step 8 - Identify Web Services

Discover web applications and management consoles.

```bash
nmap -Pn -p80,443,8080,8443 10.10.10.0/24
```

Example:

```text
10.10.10.15
10.10.10.50
10.10.10.75
```

Useful screenshots can be collected later during the Web Application Discovery phase.

### Common Discoveries

```text
Monitoring Dashboards
Backup Consoles
Virtualization Platforms
Management Portals
Internal Applications
```

---

## Step 9 - Identify Database Servers

Search for common database services.

### Microsoft SQL Server

```bash
nmap -Pn -p1433 10.10.10.0/24
```

### PostgreSQL

```bash
nmap -Pn -p5432 10.10.10.0/24
```

### MySQL

```bash
nmap -Pn -p3306 10.10.10.0/24
```

Potential findings:

```text
MSSQL01
DB01
POSTGRES-01
```