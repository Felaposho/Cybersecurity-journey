# Network Sniffing & Packet Analysis with Wireshark

## Objective
Demonstrate how unencrypted network protocols 
expose sensitive data to packet sniffing attacks

## Lab Environment
- Attacker: Kali Linux (192.168.56.102)
- Target: Metasploitable 2 (192.168.56.104)
- Tool: Wireshark

## Tools Used
- Wireshark — packet capture and analysis
- ftp — FTP client
- telnet — Telnet client

## Test 1 — FTP Credential Sniffing

### Step 1 — Start Wireshark capture on eth1
### Step 2 — Connect to FTP
```bash
ftp 192.168.56.104
```
### Step 3 — Login with credentials
```
Name: msfadmin
Password: msfadmin
```
### Step 4 — Analyze in Wireshark
- Filter: `ftp`
- Right click packet → Follow TCP Stream

### Finding
FTP transmits credentials in PLAINTEXT:
```
220 (vsFTPd 2.3.4)
USER msfadmin
331 Please specify the password.
PASS msfadmin
230 Login successful.
```
**Full credentials captured by Wireshark!** ⚠️

---

## Test 2 — Telnet Session Capture

### Step 1 — Connect via Telnet
```bash
telnet 192.168.56.104
```
### Step 2 — Login
```
login: msfadmin
password: msfadmin
```
### Step 3 — Analyze in Wireshark
- Filter: `telnet`
- Follow TCP Stream

### Finding
- Every keystroke transmitted in plaintext
- Full login session visible
- Commands executed on target visible

---

## Test 3 — HTTP Traffic Analysis

### Step 1 — Filter HTTP traffic
```
Filter: http
```
### Finding
- DVWA login POST requests visible
- Credentials in plaintext
- Full web session exposed

---

## Vulnerable Protocols Demonstrated

| Protocol | Port | Issue | Secure Alternative |
|---|---|---|---|
| FTP | 21 | Plaintext credentials | SFTP (Port 22) |
| Telnet | 23 | Plaintext everything | SSH (Port 22) |
| HTTP | 80 | Unencrypted traffic | HTTPS (Port 443) |

## Key Takeaways
1. Never use FTP, Telnet or HTTP on production systems
2. Always use encrypted alternatives
3. Network sniffing is passive — hard to detect
4. Anyone on same network segment can capture traffic
5. Wireshark is essential tool for both attackers and defenders
