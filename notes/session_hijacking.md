# Session Hijacking Lab with Burp Suite

## Objective
Demonstrate how weak session tokens can be 
intercepted and analyzed to perform session 
hijacking attacks

## Lab Environment
| Machine | IP | Role |
|---|---|---|
| Kali Linux | 192.168.56.102 | Attacker |
| Metasploitable 2 | 192.168.56.104 | Target |

## Tools Used
- Burp Suite Community Edition v2026.7.2
- DVWA (Damn Vulnerable Web Application)

---

## Step 1 — Configure Burp Suite Proxy
- Set browser proxy to 127.0.0.1:8080
- Enable Intercept in Proxy tab
- Open DVWA in Burp browser

## Step 2 — Intercept Login Request
- Navigated to http://192.168.56.104/dvwa/login.php
- Burp intercepted POST login request
- Captured session cookie in request headers:

```
Cookie: security=high; 
PHPSESSID=b8df6e587fabff1e8832e6085000d0b9
```

**Key observation:**
- Session cookie transmitted in plaintext HTTP
- No HTTPS protection
- Cookie visible to anyone on the network

## Step 3 — Session Token Analysis with Sequencer
- Right clicked PHPSESSID in intercepted request
- Sent to Burp Sequencer
- Ran live capture — 663 requests analyzed

**Sequencer Results:**
| Property | Value |
|---|---|
| Sample size | 663 tokens |
| Token length | 4 characters |
| Errors | 0 |
| Reliability | POOR |

## Key Findings

### 1. Weak Session Tokens
Token length of only 4 characters = extremely weak
Short tokens are easy to brute force or predict

### 2. Poor Randomness
Sequencer entropy analysis showed poor reliability
Tokens lack sufficient randomness making them
predictable to an attacker

### 3. No HTTPS
Session cookies transmitted over HTTP
Anyone on the network can steal the cookie
using packet sniffing (as demonstrated earlier)

### 4. No Secure Cookie Flags
Missing HttpOnly flag — cookies accessible via JavaScript
Missing Secure flag — cookies sent over HTTP

## Defensive Countermeasures

| Vulnerability | Fix |
|---|---|
| Weak token length | Use 32+ character random tokens |
| Poor randomness | Use cryptographically secure random generators |
| No HTTPS | Enforce HTTPS on all pages |
| Missing flags | Set HttpOnly and Secure cookie flags |
| No timeout | Implement session expiry after inactivity |

## Key Takeaway
Session hijacking doesn't always require exploiting 
complex vulnerabilities. Weak session token generation 
combined with unencrypted HTTP makes stealing sessions 
trivial. Burp Suite Sequencer is an essential tool 
for testing session token strength in web applications.
