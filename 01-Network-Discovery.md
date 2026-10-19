# 01 - Network Discovery

## Objective

The goal of the network discovery phase is to understand the network topology, identify reachable subnets, discover active hosts, and locate infrastructure components such as gateways, DNS servers, and domain controllers.

---

## Step 1 - Identify Current Network Configuration

```bash
ip addr show
```

Identify:

* IP Address
* Network Interface
* Subnet Mask

Example:

```text
eth0: 10.10.20.55/24
```

```bash
ip route
```

Identify:

* Default Gateway
* Accessible Subnets
* Static Routes

Example:

```text
default via 10.10.20.1

10.10.20.0/24
10.10.30.0/24
10.20.0.0/16
```

---

## Step 2 - Identify Network Neighbors

Check the ARP cache.

```bash
arp -a
```

Example:

```text
10.10.20.1
10.10.20.10
10.10.20.15
```

These hosts have recently communicated with the current machine.

---

## Step 3 - Discover Live Hosts on the Current Subnet (/24)

### Classical Discovery using nmap

```bash
nmap -sn 10.10.20.0/24
```

Example:

```text
Nmap scan report for 10.10.20.1
Nmap scan report for 10.10.20.10
Nmap scan report for 10.10.20.30
```

### Fast Discovery Using fping

```bash
fping -a -g 10.10.20.0/24 2>/dev/null
```

---

## Step 4 - Discover Additional Internal Networks

Review routing information:

```bash
ip route
```

Validate network accessibility:

```bash
nmap -sn 10.10.0.0/16
```

---

## Step 5 - Identify DNS Servers

```bash
cat /etc/resolv.conf
```

Example:

```text

```