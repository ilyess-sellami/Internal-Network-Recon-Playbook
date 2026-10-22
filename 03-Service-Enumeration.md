# 03 - Service Enumeration

## Objective

The goal of this phase is to enumerate exposed services, identify accessible resources, determine their functions, and discover security-relevant information that may lead to further investigation.

At the end of this phase, you should know:

- Which services are running
- What versions are exposed
- Which systems provide authentication services
- Which systems provide file sharing
- Which systems provide remote administration
- Which services require deeper assessment

---

## Step 1 - Full Port Scan

Start with a complete TCP port scan.

```bash
nmap -Pn -p- 10.10.10.10
```

Example:

```text
22/tcp    open ssh
53/tcp    open domain
88/tcp    open kerberos
389/tcp   open ldap
445/tcp   open microsoft-ds
```

---

## Step 2 - Service Version Detection

Identify products and versions.

```bash
nmap -Pn -sV 10.10.10.10
```

Example:

```text
22/tcp   OpenSSH 8.4
80/tcp   Apache httpd 2.4
443/tcp  nginx 1.22
```

### Reporting Notes

```text
Host: web01

SSH     OpenSSH 8.4
HTTP    Apache 2.4
HTTPS   nginx 1.22
```

---

## Step 3 - SMB Enumeration

Identify SMB-enabled systems.

### Discover SMB Hosts

```bash
nmap -Pn -p445 10.10.10.0/24
```

### Enumerate Shares

```bash
smbclient -L //10.10.10.20 -N
```

or

```bash
smbmap -H 10.10.10.20
```

Example:

```text
ADMIN$
C$
NETLOGON
SYSVOL
Shared
```

---

## Step 4 - LDAP Enumeration

Identify LDAP-enabled systems.

### Discover LDAP Services

```bash
nmap -Pn -p389,636 10.10.10.0/24
```

Example:

```text
389/tcp open ldap
636/tcp open ldaps
```

### Query Naming Contexts

```bash
ldapsearch -x -H ldap://10.10.10.10 -s base
```

Potential findings:

```text
defaultNamingContext
rootDomainNamingContext
dnsHostName
```

---

## Step 5 - Kerberos Enumeration

Identify Kerberos infrastructure.

### Discover Kerberos Services

```bash
nmap -Pn -p88 10.10.10.0/24
```

Example:

```text
88/tcp open kerberos-sec
```

---

## Step 6 - DNS Enumeration

### Discover DNS Servers

```bash
nmap -Pn -p53 10.10.10.0/24
```

### Query Records

```bash
host dc01.company.local 10.10.10.10
```

---

## Step 7 - SSH Enumeration

Discover Linux servers and appliances.

### Identify SSH Hosts

```bash
nmap -Pn -p22 10.10.10.0/24
```

Example:

```text
22/tcp open ssh
```

---

## Step 8 - HTTP / HTTPS Enumeration

Discover web applications and management consoles.

### Identify Web Servers

```bash
nmap -Pn -p80,443,8080,8443 10.10.10.0/24
```

### Retrieve HTTP Headers

```bash
curl -I http://10.10.10.20
```

Example:

```text
Server: nginx
```

### Enumerate Web Technologies

```bash
whatweb http://10.10.10.20
```

Potential findings:

```text
Apache
Nginx
IIS
Tomcat
Jenkins
GitLab
```

---

## Step 9 - RDP Enumeration

Identify remotely manageable Windows hosts.

### Discover RDP Services

```bash
nmap -Pn -p3389 10.10.10.0/24
```

Example:

```text
3389/tcp open ms-wbt-server
```

---

## Step 10 - WinRM Enumeration

Identify PowerShell remoting services.

### Discover WinRM

```bash
nmap -Pn -p5985,5986 10.10.10.0/24
```

Example:

```text
5985/tcp open wsman
```

---

## Step 11 - Database Enumeration

Identify database services.

### MSSQL

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

---

## Step 12 - NFS Enumeration

Common in Linux environments.

### Discover NFS

```bash
nmap -Pn -p111,2049 10.10.10.0/24
```

### Enumerate Exports

```bash
showmount -e 10.10.10.20
```

Example:

```text
/export/backups
/export/shared
```

---

## Step 13 - SNMP Enumeration

Network and infrastructure devices frequently expose SNMP.

### Discover SNMP

```bash
nmap -sU -p161 10.10.10.0/24
```

Potential targets:

```text
Switches
Routers
Printers
Firewalls
UPS Systems
```

---

## Step 14 - Categorize Discovered Services

```text
Authentication
├── DNS
├── LDAP
├── Kerberos

File Sharing
├── SMB
├── NFS

Remote Access
├── SSH
├── RDP
├── WinRM

Databases
├── MSSQL
├── PostgreSQL
├── MySQL

Web Services
├── HTTP
├── HTTPS

Infrastructure
├── SNMP
```