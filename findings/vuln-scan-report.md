# Vulnerability Scan Report

## Assessment Overview

This assessment was conducted in a controlled lab environment using Metasploitable2/VulnHub targets to simulate an internal network penetration test.

The objective was to identify:
- Exposed services
- Weak configurations
- Vulnerable software
- Authentication weaknesses
- Privilege escalation opportunities

---

# Target Information

| Field | Details |
|---|---|
| Target Machine | Metasploitable2 |
| Target IP | 192.168.56.102 |
| Attacker Machine | Kali Linux |
| Assessment Type | Internal Network Pentest |
| Scope | Single Host Assessment |

---

# Methodology

The following phases were performed during the assessment:

1. Network Discovery
2. Port Scanning
3. Service Enumeration
4. SMB Enumeration
5. Web Enumeration
6. Vulnerability Identification
7. Exploitation
8. Local Privilege Escalation
9. Remediation Planning

---

# Tools Used

| Tool | Purpose |
|---|---|
| Nmap | Port scanning & service detection |
| smbclient | SMB share enumeration |
| smbmap | SMB permission mapping |
| CrackMapExec | SMB authentication testing |
| Nikto | Web vulnerability scanning |
| DirBuster | Hidden directory discovery |
| LinPEAS | Privilege escalation enumeration |
| Searchsploit | Public exploit research |

---

# Network Discovery & Enumeration

## Nmap Scan

### Command Used

```bash
nmap -p- -T4 -A 192.168.56.102
```

### Key Findings

| Port | Service | Version |
|---|---|---|
| 21 | FTP | vsftpd 2.3.4 |
| 22 | SSH | OpenSSH 4.7p1 |
| 23 | Telnet | Linux telnetd |
| 80 | HTTP | Apache 2.2.8 |
| 139 | NetBIOS | Samba |
| 445 | SMB | Samba 3.0.20 |
| 3306 | MySQL | MySQL 5.0.51a-3ubuntu5 |

---

# SMB Enumeration

## smbclient Enumeration

### Command Used

```bash
smbclient -L //192.168.56.102 -N
```

### Findings

- SMB shares accessible without authentication
- Anonymous login permitted
- Sensitive shared resources exposed

---

## smbmap Enumeration

### Command Used

```bash
smbmap -H 192.168.56.102
```

### Findings

- Read access identified on multiple shares
- Weak SMB permissions observed

---

## CrackMapExec Validation

### Command Used

```bash
crackmapexec smb 192.168.56.102
```

### Findings

- SMB signing disabled
- Legacy SMB configuration detected
- Weak authentication controls identified

---

# Web Enumeration

## Nikto Scan

### Command Used

```bash
nikto -h http://192.168.56.101
```

### Findings

- Potentially dangerous HTTP methods enabled
- Apache version outdated
- Default files and directories exposed
- Missing security headers detected

---

## Directory Bruteforcing

### Tool Used
DirBuster

### Findings

- Hidden directories identified
- Administrative paths exposed
- Backup files accessible

---

# Vulnerability Findings

---

# Finding 1 — Anonymous FTP Access

## Severity
High

## Description

The FTP service allowed anonymous authentication without valid credentials.

## Impact

An attacker may access sensitive files or upload malicious content.

## Evidence

```bash
ftp 192.168.56.101
```

## Recommendation

- Disable anonymous FTP access
- Enforce strong authentication
- Restrict FTP access to authorized users only

---

# Finding 2 — SMB Misconfiguration

## Severity
High

## Description

SMB shares were accessible without authentication.

## Impact

Unauthorized users may access internal files and shared resources.

## Evidence

```bash
smbclient -L //192.168.56.101 -N
```

## Recommendation

- Disable anonymous SMB access
- Enforce SMB authentication
- Restrict share permissions

---

# Finding 3 — Outdated Services

## Severity
Critical

## Description

Multiple outdated services were identified during enumeration.

## Affected Services

- vsftpd 2.3.4
- Samba 3.0.20
- Apache 2.2.8

## Impact

Public exploits may allow remote compromise of the target system.

## Recommendation

- Upgrade vulnerable services
- Apply security patches
- Remove unsupported software

---

# Finding 4 — Web Server Misconfiguration

## Severity
Medium

## Description

Nikto identified insecure web server configurations and exposed resources.

## Impact

Information disclosure may assist attackers during reconnaissance.

## Recommendation

- Remove unnecessary files
- Disable directory listing
- Harden Apache configuration
- Implement HTTP security headers

---

# Finding 5 — Hidden Directories Exposed

## Severity
Medium

## Description

Directory brute forcing identified hidden directories and files accessible without authentication.

## Impact

Sensitive administrative or backup files may be exposed.

## Recommendation

- Restrict access to sensitive directories
- Remove unused files
- Implement proper access controls

---

# Exploitation Summary

| Vulnerability | Result |
|---|---|
| Weak SMB Configuration | Successful enumeration |
| Anonymous FTP Access | Successful access |
| Privilege Escalation | Root access obtained |

---

# Risk Summary

| Severity | Count |
|---|---|
| Critical | 1 |
| High | 2 |
| Medium | 2 |

---

# Conclusion

The assessment identified multiple high-risk vulnerabilities within the internal network environment, including outdated services, weak SMB configurations, anonymous access, and exposed directories.

Successful privilege escalation demonstrated the potential impact of these weaknesses, resulting in full system compromise.

Immediate remediation and hardening measures are recommended to reduce attack surface and improve overall security posture.

---

# Disclaimer

This assessment was conducted in an isolated lab environment for educational and ethical security testing purposes only.
