# Basic Firewall Configuration with UFW

## Objective
The objective of this task is to configure a basic firewall using UFW (Uncomplicated Firewall) on a Linux system. The firewall is configured to allow SSH traffic, deny HTTP traffic, enable the firewall, and verify that the correct rules are active.

---

# Tool Used
- UFW (Uncomplicated Firewall)
- Kali Linux / Ubuntu / Debian-based Linux

---

# Introduction

A firewall is a security mechanism that monitors and controls incoming and outgoing network traffic based on predefined rules.

UFW stands for **Uncomplicated Firewall**. It is a simple and user-friendly command-line tool used to manage Linux firewall rules. Internally, UFW works with iptables or nftables but provides an easier interface for beginners and administrators.

Using UFW, we can:
- Allow trusted traffic
- Block unauthorized traffic
- Restrict access to ports
- Improve system security

---

# Why Firewall Configuration is Important

Firewall rules are important because they help to:

- Prevent unauthorized access
- Reduce attack surface
- Protect services running on the system
- Control which ports are reachable
- Improve overall cybersecurity posture

---

# Environment Details

- Operating System: Kali Linux
- Firewall Tool: UFW
- Rules Applied:
  - Allow SSH
  - Deny HTTP

---

# Step 1: Update Package List

## Command Executed

```bash
sudo apt update
```

## Output

```text
Get:1 http://kali.download/kali kali-rolling InRelease [34.0 kB]
Fetched 74.4 MB in 15s (5,005 kB/s)
2680 packages can be upgraded.
```

## Explanation

This command refreshes package information from online repositories.

### Meaning of Output
- **Get:** Repository metadata downloaded
- **Fetched 74.4 MB:** Data downloaded successfully
- **2680 packages can be upgraded:** Updates are available on the system

---

# Step 2: Install UFW

## Command Executed

```bash
sudo apt install ufw -y
```

## Output

```text
Installing:
  ufw

Summary:
  Upgrading: 0, Installing: 1, Removing: 0

Setting up ufw (0.36.2-9) ...
Created symlink ...
```

## Explanation

This installs the UFW package.

### Meaning of Output
- **Installing: ufw** → UFW package selected
- **Installing: 1** → One new package installed
- **Setting up ufw** → Configuration completed
- **Created symlink** → UFW enabled for startup

---

# Step 3: Allow SSH Traffic

## Command Executed

```bash
sudo ufw allow ssh
```

## Output

```text
Rules updated
Rules updated (v6)
```

## Explanation

SSH stands for Secure Shell. It is used for secure remote login and administration.

By allowing SSH:
- Port **22** becomes accessible
- Remote administration remains possible
- Encrypted management access is preserved

### Why This Rule is Important
If SSH is blocked, administrators may lose remote access to the machine.

---

# Step 4: Deny HTTP Traffic

## Command Executed

```bash
sudo ufw deny http
```

## Output

```text
Rules updated
Rules updated (v6)
```

## Explanation

HTTP uses port **80** and is used for websites.

By denying HTTP:
- Incoming web traffic is blocked
- Unnecessary web exposure is prevented
- Attack surface is reduced

---

# Step 5: Enable Firewall

## Command Executed

```bash
sudo ufw enable
```

## Output

```text
Firewall is active and enabled on system startup
```

## Explanation

This command activates the firewall immediately.

### Meaning of Output
- **Firewall is active** → Rules are currently enforced
- **Enabled on system startup** → Firewall starts automatically after reboot

---

# Step 6: Check Firewall Status

## Command Executed

```bash
sudo ufw status numbered
```

## Output

```text
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22/tcp                     ALLOW IN    Anywhere
[ 2] 80/tcp                     DENY IN     Anywhere
[ 3] 22/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 4] 80/tcp (v6)                DENY IN     Anywhere (v6)
```

---

# Detailed Explanation of Output

## Status: active
The firewall is currently running and protecting the system.

## Column Meaning

### To
Destination port or service.

### Action
What happens to traffic:
- **ALLOW** = Permit connection
- **DENY** = Block connection

### From
Source of incoming traffic.

**Anywhere** means any IP address can attempt access.

---

# Rule-by-Rule Explanation

## Rule [1]
`22/tcp ALLOW IN Anywhere`

Allows IPv4 SSH traffic on port 22.

## Rule [2]
`80/tcp DENY IN Anywhere`

Blocks IPv4 HTTP traffic on port 80.

## Rule [3]
`22/tcp (v6) ALLOW IN Anywhere (v6)`

Allows IPv6 SSH traffic.

## Rule [4]
`80/tcp (v6) DENY IN Anywhere (v6)`

Blocks IPv6 HTTP traffic.

---

# Networking Concepts

## What is a Port?

A port is a communication endpoint used by applications.

Examples:
- Port 22 → SSH
- Port 80 → HTTP
- Port 443 → HTTPS

---

## What is TCP?

TCP (Transmission Control Protocol) is a reliable communication protocol used by many network services.

Examples:
- SSH uses TCP
- HTTP uses TCP
- HTTPS uses TCP

---

## Why Separate IPv4 and IPv6 Rules?

Modern systems support both:
- IPv4
- IPv6

UFW creates rules for both versions to ensure complete protection.

---

# Security Significance

## SSH Allowed

Good because:
- Required for system administration
- Secure encrypted access
- Standard management protocol

Best Practices:
- Use SSH keys
- Disable root login
- Use strong passwords

---

## HTTP Denied

Good because:
- Stops unnecessary website access
- Prevents accidental exposure
- Reduces web attack risks

---

# Automation Script

The file `ufw_configuration.sh` automates the complete setup:

1. Update packages  
2. Install UFW  
3. Allow SSH  
4. Deny HTTP  
5. Enable firewall  
6. Show firewall status  

## Run Script

```bash
chmod +x ufw_configuration.sh
./ufw_configuration.sh
```

---

# Files Included

- `ufw_configuration.sh`
- `README.md`
- Screenshot of `ufw status numbered`

---

# Conclusion

This task successfully demonstrated how to configure a Linux firewall using UFW. SSH traffic was allowed, HTTP traffic was denied, and the firewall was enabled with verified rules. UFW is a simple yet powerful tool for securing Linux systems and controlling network access.