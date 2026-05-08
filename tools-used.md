# Tools Used

## Nmap
Used for:
- Host discovery
- Port scanning
- Service/version detection

Command:
1. full TCP port scan:
```bash
 nmap -p- -T4 TARGET_IP
```
2. Version + Default scripts
```bash
nmap -sV -sC -oA TARGET_IP
```

---

## smbclient

Used to enumerate SMB shares.

Command:
```bash
smbclient -L //TARGET_IP -N
```

---

## smbmap

Used to identify SMB permissions and accessible shares.

Command:
```bash
smbmap -H TARGET_IP
```
## CrackMapExec

Used for:
- SMB authentication testing
- Share enumeration
- Credential validation
- Lateral movement simulation

Command:
```bash
crackmapexec smb TARGET_IP
```
## Nikto

Used for:
- Web server vulnerability scanning
- Dangerous file detection
- Misconfiguration discovery

Command:
```bash
nikto -h http://TARGET_IP
```
## DirBuster

Used for:
- Hidden directory discovery
- File enumeration
- Web content mapping

Common wordlist:
```bash
/usr/share/wordlists/dirbuster/directory-list
```

Example:
```bash
dirbuster&
```
---

## LinPEAS

Used for Linux privilege escalation enumeration.

Command:
```bash
./linpeas.sh
```

---

## Searchsploit

Used to research public exploits.

Command:
```bash
searchsploit service_name
```
