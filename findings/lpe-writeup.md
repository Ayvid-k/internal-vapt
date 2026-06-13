# Local Privilege Escalation Writeup

## Overview

This document describes the privilege escalation process performed during the internal network penetration test conducted against the Metasploitable2 target machine.

The objective was to escalate privileges from a low-privileged user account to root access after obtaining an initial foothold on the target system.

---

# Target Information

| Field | Details |
|---|---|
| Target Machine | Metasploitable2 |
| Target IP | 192.168.x.x |
| Operating System | Linux |
| Assessment Type | Internal Pentest |
| Service | Telnet |

---

# Objective

- Gain initial shell access
- Enumerate privilege escalation vectors
- Identify system misconfigurations
- Escalate privileges to root
- Document exploitation steps
- Provide remediation guidance

---
# Attack Chain Overview (Internal Network Pentest)

This section outlines the complete attack path followed during the internal penetration test, from initial access to full system compromise.

---

# Attack Flow

```text
Telnet Misconfiguration
        ↓
Weak/Default Credential Login
        ↓
Initial Shell Access (www-data)
        ↓
System Enumeration (uname, id, netstat)
        ↓
LinPEAS Execution
        ↓
Discovery of Privilege Escalation Vectors
        ↓
SUID / Kernel Exploit Path
        ↓
Privilege Escalation
        ↓
Root Access
```
## 1. Reconnaissance

- Nmap scanning performed
- Open ports identified (including Telnet, SMB, FTP, HTTP)
- Service version detection enabled attack surface mapping

---

## 2. Initial Access (Telnet)

- Legacy Telnet service exposed on port 23
- Weak/default credentials identified
- Successful login achieved
- Low-privileged shell obtained
---

### Verification Command

```bash
whoami
```

### Output

```bash
msfadmin
```
The session was established with low-privileged user access, requiring further privilege escalation.

## 3. Post-Exploitation Enumeration

- System information gathered using `uname -a`
- User context identified (`www-data`)
- Running services enumerated
- Internal attack surface mapped

Post-exploitation enumeration was performed to identify privilege escalation opportunities.

---

## Kernel Information

### Command Used

```bash
uname -a
```

### Output

```bash
Linux metasploitable 2.x.x-16-server #1 SMP Thu Apr 10 13:58:00 UTC 2008 i686 GNU/Linux
```

The kernel version was identified as outdated and potentially vulnerable to known local privilege escalation exploits.

---

## 4. Privilege Escalation Discovery

- LinPEAS executed for automated analysis
- SUID binaries identified
- Outdated kernel version detected
- Misconfigurations discovered

---

## Command Used

```bash
chmod +x linpeas.sh
./linpeas.sh
```
---

# Key Findings

LinPEAS identified multiple potential privilege escalation vectors including:

- Outdated Linux kernel
  ```bash
  Linux version 2.x.x-16-server (buildd@palmer) (gcc version 4.2.3 (Ubuntu 4.2.3-2ubuntu7)) #1 SMP Thu Apr 10 13:58:00 UTC 2008                                                                                                             
  ```
- Misconfigured SUID binaries
 
- Weak file permissions
  
- Legacy software versions

---

# SUID Enumeration

### Command Used

```bash
find / -perm -4000 2>/dev/null
```

### Example Output

```bash
/bin/umount
/bin/fusermount
/bin/su
/bin/mount
/bin/ping
/bin/ping6
etc....
```
Improperly configured or vulnerable SUID binaries may allow attackers to escalate privileges to root.red.


# Writable Directory Checks

Writable directories were identified to determine whether malicious files or scripts could be placed for privilege escalation.

### Command Used

```bash
find / -writable -type d 2>/dev/null
```

---

## 5. Privilege Escalation

- Exploit path identified using public research tools
- Local privilege escalation executed successfully
- Root access achieved

### Command Used

```bash
searchsploit linux kernel 2.x privilege escalation
```
---
### Observation

Multiple public privilege escalation references affecting the identified kernel family were discovered.

This confirmed that the outdated kernel significantly increased the risk of local privilege escalation.

### Exploit Research
![SearchSploit](screenshots/searchsploit-results.png)

---
# Root Access Verification

Following successful privilege escalation, administrative access to the target system was verified.

### Command Used

```bash
whoami
```

### Output

```bash
root
```

---

# Root Access Achieved

The target system was fully compromised with administrative/root privileges.

---

# Security Impact

Successful privilege escalation allows an attacker to:

- Gain complete system control
- Access sensitive files
- Modify system configurations
- Disable security mechanisms
- Install persistence mechanisms
- Move laterally across the network

This significantly increases the overall impact of the compromise.

---

# Risk Rating

| Metric | Value |
|---|---|
| Likelihood | High |
| Impact | Critical |
| Overall Risk | Critical |

---

# Remediation Recommendations

## Patch Management

- Upgrade outdated Linux kernels
- Apply latest security updates
- Remove unsupported software

---

## SUID Hardening

Review unnecessary SUID binaries:

```bash
find / -perm -4000 2>/dev/null
```

Remove or restrict dangerous binaries where possible.

---

## Least Privilege

- Restrict unnecessary user permissions
- Minimize service account privileges
- Implement role-based access controls

---

## Monitoring

- Monitor privilege escalation attempts
- Enable centralized logging
- Deploy endpoint detection solutions

---

# Lessons Learned

This assessment demonstrated how:
- Outdated systems increase risk exposure
- Weak configurations enable escalation paths
- Proper patch management is critical
- Post-exploitation enumeration is essential during pentests

---

# Conclusion

The target system contained multiple privilege escalation vectors due to outdated software and insecure configurations.

Through systematic enumeration and exploit research, root-level access was successfully achieved, demonstrating the potential impact of an attacker gaining an initial foothold within the internal network.

Immediate remediation and system hardening are strongly recommended.

---

# Disclaimer

This activity was conducted in a controlled lab environment for educational and authorized security testing purposes only.
