# Basic Network Scanning with Nmap

## Objective
The objective of this task is to perform a basic network scan using Nmap to identify reachable hosts, detect open ports, and understand exposed network services.

## Tool Used
- Nmap (Network Mapper)
- Kali Linux

## Introduction
Nmap is a widely used open-source network scanning tool used by system administrators and cybersecurity professionals to discover hosts, identify open ports, detect running services, and assess firewall rules.

Port scanning is important because each service communicates through a port number. If a port is exposed, it may provide access to applications such as web servers, remote login, or databases.

---

## Environment Details
- Interface: eth0
- Target IP Address: 192.168.88.131
- Network Range: 192.168.88.0/24
- Operating System: Kali Linux

---

# Step 1: Installing Nmap

## Command
```bash
sudo apt install nmap -y
```

## Purpose
This command installs or upgrades Nmap and automatically confirms prompts using the `-y` option.

## Result
Nmap was successfully upgraded to version **7.99**.

---

# Step 2: Identifying the Local IP Address

## Command
```bash
ifconfig
```

## Important Output
```text
eth0: inet 192.168.88.131
```

## Explanation
The `ifconfig` command displays network interface configuration.  
The active adapter `eth0` has the IPv4 address `192.168.88.131`, which is used as the scan target.

---

# Step 3: Basic Nmap Scan

## Command
```bash
nmap 192.168.88.131
```

## Output
```text
Starting Nmap 7.99 at 2026-04-27 18:36 +0530
Nmap scan report for 192.168.88.131
Host is up.
All 1000 scanned ports on 192.168.88.131 are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)
Nmap done: 1 IP address (1 host up) scanned in 201.86 seconds
```

## Explanation of Output
- **Host is up**: The target machine is reachable.
- **1000 scanned ports**: Nmap checked the top 1000 common TCP ports.
- **Filtered ports**: Packets were blocked or dropped, likely by a firewall.
- **No open ports found**: No commonly accessible TCP services were exposed.

---

# Step 4: Service Version Detection

## Command
```bash
nmap -sV 192.168.88.131
```

## Purpose
The `-sV` option attempts to identify services and software versions running on open ports.

## Output Summary
No services were detected because no open ports were found.

## Interpretation
If ports were open, this scan could reveal services such as:
- Apache Web Server
- OpenSSH
- MySQL
- FTP Server

This helps administrators verify what software is exposed.

---

# Step 5: Fast Scan

## Command
```bash
nmap -F 192.168.88.131
```

## Purpose
The `-F` option scans only the most common 100 ports, making the scan faster.

## Output
```text
Host is up.
All 100 scanned ports are filtered.
```

## Explanation
The fast scan confirmed the same behavior as the full scan but completed much faster.

---

# Understanding Port States

## Open
An application is actively listening on the port.

## Closed
The port is reachable, but no service is listening.

## Filtered
A firewall or security device blocks probes, preventing detection.

---

# Common Ports and Their Importance

| Port | Service | Use Case | Security Risk |
|------|---------|----------|---------------|
| 22 | SSH | Remote administration | Brute-force login attempts |
| 80 | HTTP | Website hosting | Web attacks |
| 443 | HTTPS | Secure web traffic | Misconfiguration |
| 21 | FTP | File transfer | Weak credentials |
| 25 | SMTP | Email transfer | Spam abuse |
| 3306 | MySQL | Database access | Data leakage |
| 3389 | RDP | Remote desktop | Credential attacks |

---

# Why No Open Ports Were Found

Possible reasons:
1. Host firewall is enabled.
2. No network services are running.
3. VM network adapter blocks inbound requests.
4. Security rules drop scan packets.

This is often a positive sign because fewer exposed services reduce attack surface.

---

# Security Recommendations

- Keep unused ports closed.
- Use strong firewall rules.
- Expose services only when necessary.
- Regularly perform internal scans.
- Keep all software updated.

---

# Files Included

- `nmap_scan_results.txt` – Raw command outputs
- `README.md` – Detailed explanation
- Screenshots – Proof of execution

---

# Conclusion

This task demonstrated how to install Nmap, identify a local IP address, and perform multiple scans. Even though no open ports were discovered, the results are valuable because they confirm host availability and indicate active filtering controls. Nmap remains one of the most important tools for network auditing and cybersecurity assessment.
