# Basic Network Scanning with Nmap

## Objective
The objective of this task is to perform a basic network scan using Nmap to identify reachable hosts, detect open ports, and understand exposed network services. This exercise teaches fundamental network reconnaissance techniques used in cybersecurity assessments.

## Tool Used
- **Nmap** (Network Mapper) - Open-source network scanning tool
- **Kali Linux** - Linux distribution for penetration testing
- **Network Interface:** eth0 (Ethernet adapter)

## Introduction

Nmap is a widely used open-source network scanning tool used by system administrators and cybersecurity professionals to discover hosts, identify open ports, detect running services, and assess firewall rules. Understanding how to use Nmap is crucial for:

- **Network Auditing** - Discovering devices on a network
- **Security Assessment** - Identifying exposed services
- **Vulnerability Management** - Baseline security checks
- **Compliance** - Verifying security policies are enforced

Port scanning is important because each service communicates through a port number. If a port is exposed, it may provide access to applications such as web servers (port 80), remote login (port 22), or databases (port 3306).

**Key Concepts:**
- Ports act as entry points for network services
- Each TCP/UDP port can be in one of several states: Open, Closed, or Filtered
- Open ports indicate running services that could be potential targets

---

## Environment Details
| Property | Value |
|----------|-------|
| **Interface** | eth0 |
| **Target IP Address** | 192.168.88.131 |
| **Network Range** | 192.168.88.0/24 |
| **Operating System** | Kali Linux |
| **Nmap Version** | 7.99 |

---

# Step 1: Installing Nmap

## Command
```bash
sudo apt install nmap -y
```

## Detailed Explanation

- **sudo** - Executes command with administrative privileges (required for system-wide installation)
- **apt** - Advanced Package Tool, Debian/Ubuntu package manager
- **install** - Package management action
- **nmap** - The package name to install
- **-y** - Automatically confirms prompts without user interaction

## Purpose
This command installs or upgrades Nmap and automatically confirms prompts using the `-y` option, allowing for automated deployment.

## Result
Nmap was successfully upgraded to version **7.99**, enabling network scanning capabilities.

---

# Step 2: Identifying the Local IP Address

## Purpose
Before scanning a network, we need to identify our own machine's IP address and the target network range. This helps in:
- Understanding the network topology
- Determining which networks to scan
- Configuring firewall rules appropriately

## Command
```bash
ifconfig
```

## Important Output
```text
eth0: inet 192.168.88.131
        netmask 255.255.255.0
        broadcast 192.168.88.255
```

## Detailed Explanation
The `ifconfig` command displays network interface configuration information:

- **eth0** - Ethernet interface name
- **inet 192.168.88.131** - IPv4 address of this machine
- **netmask 255.255.255.0** - Subnet mask (defines which IPs are on same network)
- **broadcast 192.168.88.255** - Broadcast address for the network

The active adapter `eth0` has the IPv4 address `192.168.88.131`, which is used as the scan target in this exercise.

**Network Analysis:**
- Network Address: 192.168.88.0
- Usable IPs: 192.168.88.1 - 192.168.88.254
- Our Machine IP: 192.168.88.131
- Total Hosts: 254

---

# Step 3: Basic Nmap Scan

## Purpose
Perform a standard port scan on the target IP to identify open ports and services. This is the foundation of network reconnaissance.

## Command
```bash
nmap 192.168.88.131
```

## Full Output
```text
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-27 18:36 +0530
Nmap scan report for 192.168.88.131
Host is up.
All 1000 scanned ports on 192.168.88.131 are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)

Nmap done: 1 IP address (1 host up) scanned in 201.86 seconds
```

**Screenshot Reference:** ![Basic Nmap Scan](Screenshots/nmap\ 192.168.88.131.png)

## Detailed Explanation of Output

### Host Status
- **Host is up** → The target machine is reachable on the network

### Port Information
- **1000 scanned ports** → Nmap checked the top 1000 common TCP ports by default
- **Filtered ports** → Packets sent to the target were blocked or dropped, likely by a firewall
- **No open ports found** → No commonly accessible TCP services were exposed

### Timing
- **Scan duration: 201.86 seconds** → Relatively long time indicates firewall is actively filtering responses

### Security Implication
The presence of filtered ports indicates:
- ✅ Firewall is active and working correctly
- ✅ Unnecessary ports are not exposed
- ✅ System is following security best practices

---

# Step 4: Service Version Detection Scan

## Purpose
Attempt to identify specific software versions running on open ports. This helps determine:
- What services are running
- Potential vulnerabilities based on version
- Whether services are outdated

## Command
```bash
nmap -sV 192.168.88.131
```

## Detailed Explanation

**Flag Breakdown:**
- **-sV** (Version Detection) - Probes open ports to determine application name and version

## Output Summary
```text
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-27 18:43 +0530
Nmap scan report for 192.168.88.131
Host is up.
```

**Screenshot Reference:** ![Service Version Scan](Screenshots/nmap\ -sV\ 192.168.88.131.png)

## Why No Services Were Detected

Since no open ports were found in Step 3, no services can be detected in this step. However, if ports were open, this scan could reveal:

| Service | Port | Common Version | Risk |
|---------|------|----------------|------|
| Apache Web Server | 80, 443 | 2.4.x | Unpatched versions may have vulnerabilities |
| OpenSSH | 22 | 7.4+ | Older versions prone to brute-force attacks |
| MySQL / MariaDB | 3306 | 5.7 | Default passwords, unpatched databases |
| FTP Server | 21 | ProFTPD | Weak authentication mechanisms |
| Postfix Mail Server | 25 | 3.x | Email spoofing, spam abuse |

This scan helps administrators verify what software is exposed and determine if any are running outdated versions.

---

# Step 5: Fast Scan (Top 100 Ports)

## Purpose
Perform a quicker scan by checking only the most commonly used ports. Useful when:
- Time is limited
- Only need to identify major exposed services
- Performing routine checks

## Command
```bash
nmap -F 192.168.88.131
```

## Detailed Explanation

**Flag Breakdown:**
- **-F** (Fast Mode) - Scans only the top 100 most common ports instead of the default 1000

## Output
```text
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-27 18:50 +0530
Nmap scan report for 192.168.88.131
Host is up.
All 100 scanned ports are filtered.
Nmap done: 1 IP address (1 host up) scanned in 45.32 seconds
```

**Screenshot Reference:** ![Fast Scan Results](Screenshots/nmap\ -F\ 192.168.88.131.png)

## Comparison of Scan Methods

| Scan Type | Ports Checked | Time | Use Case |
|-----------|---------------|------|----------|
| Standard (nmap IP) | Top 1000 | ~3 min | General network assessment |
| Fast (-F) | Top 100 | ~45 sec | Quick checks, routine scans |
| Service Detection (-sV) | Open ports only | Varies | Identify running services |
| Comprehensive (-p-) | All 65535 | Hours | Deep security audits |

The fast scan confirmed the same behavior as the full scan but completed much faster (45 seconds vs 201 seconds), demonstrating the trade-off between speed and thoroughness.

---

# Understanding Port States in Detail

## Port States

### 1. **OPEN** ✅
- An application is actively listening on this port
- Connection is accepted
- Potential security risk if service is unnecessary
- Example: Port 80 (HTTP) on a web server

### 2. **CLOSED** ⚠️
- The port is reachable on the target machine
- No service is listening on this port
- Host responds with TCP RST packet
- Not as critical as open ports but indicates services could be started

### 3. **FILTERED** 🔒
- Status cannot be determined
- Firewall or security device blocks probes
- No response received from target
- Cannot tell if port is open or closed
- Indicates security controls are active

### 4. **UNFILTERED** ⚪
- Port is reachable but Nmap cannot determine if it's open or closed
- Only seen with TCP ACK scan type
- Rare in normal scans

### 5. **OPEN|FILTERED** ❓
- Nmap cannot determine the exact state
- Likely indicates firewall between scanner and target

### 6. **CLOSED|FILTERED** ❓
- Cannot be reliably determined
- Indicates filtering on response path

---

# Common Ports and Their Importance

| Port | Protocol | Service | Typical Use | Security Risk |
|------|----------|---------|-------------|---------------|
| **20-21** | TCP | FTP | File Transfer | Weak authentication, unencrypted data |
| **22** | TCP | SSH | Remote Administration | Brute-force attacks, weak keys |
| **25** | TCP | SMTP | Email Transfer | Spam relaying, spoofing |
| **53** | TCP/UDP | DNS | Domain Resolution | DNS poisoning, information leakage |
| **80** | TCP | HTTP | Website Hosting | Web attacks, MITM, XSS, SQLi |
| **110** | TCP | POP3 | Email Retrieval | Weak credentials, unencrypted |
| **143** | TCP | IMAP | Email Access | Unencrypted communication |
| **443** | TCP | HTTPS | Secure Web | Misconfiguration, weak certificates |
| **3306** | TCP | MySQL | Database Access | SQL Injection, weak credentials |
| **3389** | TCP | RDP | Remote Desktop | Credential attacks, brute-force |
| **5432** | TCP | PostgreSQL | Database Access | Unencrypted connections |
| **5984** | TCP | CouchDB | NoSQL Database | No authentication by default |
| **8080** | TCP | HTTP Alt | Web Services | Unpatched web services |
| **27017** | TCP | MongoDB | NoSQL Database | No authentication by default |

---

# Why No Open Ports Were Found - Analysis

## Possible Reasons

1. **✅ Host Firewall is Enabled**
   - Most likely reason (indicated by "filtered" status)
   - Firewall is actively dropping probe packets
   - Good security practice

2. **⚠️ No Network Services Running**
   - System may be running minimal services
   - Services may be listening on non-standard ports
   - Development/test machine configuration

3. **🔒 VM Network Adapter Configuration**
   - Virtual machine network settings may restrict traffic
   - NAT or isolated network mode enabled
   - Firewall rules on hypervisor blocking traffic

4. **🛡️ Security Rules Drop Scan Packets**
   - Advanced firewall rules filtering reconnaissance attempts
   - Intrusion Detection System (IDS) may block scans
   - Network-level filtering (corporate environment)

## Assessment

This result is actually **positive from a security perspective** because:
- ✅ Fewer exposed services = smaller attack surface
- ✅ Firewall is actively filtering = defense in depth working
- ✅ No immediate obvious vulnerabilities to exploit
- ✅ Host appears to be configured securely

---

# Security Recommendations for Administrators

1. **Keep Unused Ports Closed**
   - Disable services not needed
   - Close all non-essential ports
   - Regular port audits

2. **Use Strong Firewall Rules**
   - Default deny policy
   - Allow only necessary traffic
   - Implement egress filtering

3. **Expose Services Only When Necessary**
   - Use VPN for remote access
   - Implement IP whitelisting
   - Use bastion hosts/jump servers

4. **Regularly Perform Internal Scans**
   - Monthly or quarterly network audits
   - Baseline creation and comparison
   - Track changes over time

5. **Keep All Software Updated**
   - Regular patching schedule
   - Monitor security advisories
   - Automate updates where possible

6. **Monitor Access Logs**
   - Review failed connection attempts
   - Implement intrusion detection
   - Create security alerts

---

# Nmap Scan Flags Reference

| Flag | Description | Use Case |
|------|-------------|----------|
| `-sS` | SYN Scan (Stealth) | Default TCP scan, doesn't complete handshake |
| `-sU` | UDP Scan | Discover UDP services |
| `-sV` | Version Detection | Identify service versions |
| `-O` | OS Detection | Determine target operating system |
| `-A` | Aggressive Scan | OS, version, script, traceroute (all-in-one) |
| `-p-` | Scan All Ports | Check all 65535 TCP ports |
| `-p 80,443` | Scan Specific Ports | Check only specified ports |
| `-F` | Fast Scan | Only top 100 most common ports |
| `--top-ports 20` | Top N Ports | Scan only top N ports |
| `-T4` | Timing Profile | Faster scanning (T5 = insane, T1 = paranoid) |
| `--script` | NSE Scripts | Run Nmap Scripting Engine scripts |

---

# Files Included in This Task

- **nmap_scan_results.txt** - Raw command outputs from all scans
- **README.md** - This detailed explanation document
- **Screenshots/** - Visual proof of execution
  - `nmap 192.168.88.131.png` - Standard port scan output
  - `nmap -sV 192.168.88.131.png` - Service detection scan
  - `nmap -F 192.168.88.131.png` - Fast scan results

---

# Key Takeaways

1. **Nmap is Essential** for network security professionals
2. **Port Scanning** is the first step in reconnaissance
3. **Firewall Presence** is important security control
4. **Understanding Port States** is crucial for interpretation
5. **Regular Scanning** helps maintain security posture

---

# Conclusion

This task demonstrated how to install Nmap, identify a local IP address, and perform multiple network scans using different techniques. Even though no open ports were discovered, the results are highly valuable because they confirm:

- ✅ Host availability and reachability
- ✅ Active filtering controls are in place
- ✅ Security configurations are effective
- ✅ Baseline for future security assessments

Nmap remains one of the most important tools for network auditing, vulnerability assessment, and cybersecurity assessments. Understanding its capabilities and outputs is essential for any security professional.
