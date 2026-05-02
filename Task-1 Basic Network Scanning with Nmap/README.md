# Basic Network Scanning with Nmap

## Objective
The objective of this task is to perform a basic network scan using Nmap to identify reachable hosts, detect open ports, and understand exposed network services. This exercise teaches fundamental network reconnaissance techniques used in cybersecurity assessments.

## Video Demonstration
Watch the complete walkthrough: [Task-1 Video](https://drive.google.com/file/d/19VGN9xcRXe6KhbhAWQBFnMyRbV7uOR4v/view?usp=sharing)

## Next Steps
- Review the scan results and identify whether ports are exposed or filtered.
- Determine whether the network firewall is working as expected.
- Use the findings to prioritize service hardening and further targeted scans.

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

# Security Impact Assessment & Implications

## Implications of Findings

### Current State Analysis

The scan results indicate **filtered ports rather than open ports**, which has both positive and negative implications:

#### Positive Implications ✅
1. **Host is Protected**
   - Firewall actively filtering inbound traffic
   - No obvious exposed services
   - Initial security controls are in place

2. **Reduced Attack Surface**
   - Fewer entry points for attackers
   - Limited reconnaissance data for adversaries
   - Standard security posture

3. **Compliance Alignment**
   - Follows principle of "default deny"
   - Consistent with security frameworks (NIST, ISO 27001)
   - Good baseline for further hardening

#### Negative Implications ⚠️
1. **Unknown Service State**
   - Cannot verify if necessary services are running
   - Potential services hidden behind firewall
   - May hide configuration issues

2. **Limited Visibility**
   - Cannot assess service versions for vulnerabilities
   - Unable to perform version-based risk assessment
   - May require additional testing methods

3. **Firewall Configuration Concerns**
   - May be filtering legitimate administrative access
   - Could impact remote management capabilities
   - Risk of security theater (filtering without purpose)

---

## Remediation Steps & Best Practices

### Step 1: Define Service Requirements ⚙️

**Action Items:**
- Document which services MUST be exposed
- Classify services by security level
- Define necessary ports for each service

**Example Service Inventory:**
```
Service         Port    Protocol  Necessity   Exposed?
SSH             22      TCP       Required    Only for admins
HTTP            80      TCP       No          Block
HTTPS           443     TCP       Maybe       For web services
DNS             53      UDP/TCP   Maybe       For internal only
MySQL           3306    TCP       No          Internal only
```

**Remediation Command:**
```bash
# Document current open ports (if any found after filter changes)
nmap -p- <target> > port_inventory.txt
# Verify each open port is intentional
```

---

### Step 2: Implement Principle of Least Privilege 🔐

**Action Items:**
- Close ALL ports by default
- Explicitly allow only necessary ports
- Document rationale for each allowed port
- Review and update quarterly

**Implementation Strategy:**
```bash
# Using UFW (recommended for Linux)
sudo ufw default deny incoming
sudo ufw allow ssh    # Only for authorized IPs ideally
sudo ufw enable

# Verify rules
sudo ufw status numbered
```

---

### Step 3: Segment Network by Function 🏢

**Action Items:**
- Separate administrative network from user network
- Isolate database servers
- Create DMZ for public-facing services
- Implement VLANs if applicable

**Network Segmentation Example:**
```
                    Internet
                        ↓
        ┌─────────────────────────────┐
        │    Firewall/Gateway         │
        └──────────┬────────────────────┘
                   │
        ┌──────────┼──────────┐
        │          │          │
    DMZ         Internal   Database
   Network      Network      Network
    (Public)   (Trusted)   (Restricted)
  - Web Server  - Admin   - MySQL
  - Mail        - Users   - Backup
```

---

### Step 4: Implement Remote Access Securely 🌐

**Current Problem:**
- No remote access is possible due to filtering

**Secure Solution:**
```bash
# Allow SSH only from specific IP ranges
sudo ufw allow from 203.0.113.0/24 to any port 22

# Or use bastion host pattern
sudo ufw allow in on wlan0 to any port 22
```

**Alternative: VPN Access**
```bash
# Setup VPN for remote management
# - OpenVPN
# - WireGuard (modern, faster)
# Then restrict SSH to VPN interface only
```

---

### Step 5: Enable Logging and Monitoring 📊

**Action Items:**
- Log all connection attempts
- Monitor for scanning patterns
- Alert on suspicious activity

**Logging Configuration:**
```bash
# Check UFW logs for blocked traffic
sudo tail -f /var/log/ufw.log

# Check network logs for reconnaissance attempts
sudo grep UFW /var/log/syslog | tail -20
```

**Monitoring for Reconnaissance:**
```bash
# Multiple port connections in short time = scanning
# Setup alert if >10 ports probed in 60 seconds
# Monitor for: SYN floods, UDP floods, protocol probes
```

---

### Step 6: Regular Vulnerability Assessment 🔍

**Action Items:**
- Repeat Nmap scans monthly
- Compare against baseline
- Document any changes
- Investigate deviations

**Assessment Schedule:**
```bash
# Create baseline
nmap -sV -O <target> > baseline_scan.txt

# Monthly verification
nmap -sV -O <target> > current_scan.txt
diff baseline_scan.txt current_scan.txt

# Quarterly deep scan
nmap -p- -A <target> > quarterly_scan.txt
```

---

### Step 7: Incident Response Readiness 🚨

**If Scan Results Change (Ports Open):**

1. **Immediate Actions:**
   ```bash
   # Identify what opened the port
   sudo lsof -i :<port_number>
   
   # Check running services
   sudo systemctl list-units --type=service --all
   
   # Review recent changes
   sudo journalctl -n 50
   ```

2. **Investigation:**
   - Determine if intentional
   - Check service configuration
   - Review access logs
   - Verify service legitimacy

3. **Response:**
   - Close unauthorized ports
   - Update firewall rules
   - Review system for compromise
   - Document root cause

---

## Detailed Risk Analysis

### Risk Assessment Matrix

| Finding | Probability | Impact | Severity | Mitigation |
|---------|-------------|--------|----------|-----------|
| Filtered ports bypass | Low | Medium | LOW | Maintain firewall |
| Service hidden issue | Medium | Medium | MEDIUM | Enhanced monitoring |
| Firewall misconfiguration | Low | High | MEDIUM | Regular audits |
| Unauthorized service startup | Medium | High | HIGH | Process hardening |
| Network reconnaissance | High | Low | MEDIUM | Rate limiting |

---

## Recommendations Summary

### Immediate Actions (24 hours)
- ✅ **Document current firewall state** - Create baseline for comparison
- ✅ **Verify firewall rules are intentional** - Review UFW/iptables configuration
- ✅ **Enable firewall logging** - Begin tracking blocked connections

### Short-term Actions (1 week)
- 🔄 **Implement role-based access** - Separate admin and user access
- 🔄 **Setup VPN for remote management** - Enable secure remote access if needed
- 🔄 **Create monitoring alerts** - Alert on reconnaissance attempts

### Long-term Actions (ongoing)
- 📅 **Monthly scans** - Compare against baseline
- 📅 **Quarterly reviews** - Assess if ports/services remain necessary
- 📅 **Annual security audit** - Full penetration test
- 📅 **Continuous improvement** - Update based on new threats

---

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
