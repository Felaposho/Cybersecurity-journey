# Vulnerability Scanning with Nessus Essentials

## Objective
Use Nessus to perform automated vulnerability 
scanning on a target and analyze findings

## Lab Environment
| Machine | IP | Role |
|---|---|---|
| Kali Linux | 192.168.56.102 | Attacker |
| Metasploitable 2 | 192.168.56.104 | Target |

## Tool Used
- Tenable Nessus Essentials
- Policy: Web Application Tests
- Severity Base: CVSS v3.0
- Scanner: Local Scanner

## Steps

### Step 1 — Launch Nessus
```
Access Nessus at: https://127.0.0.1:8834
```

### Step 2 — Create New Scan
- Click New Scan
- Select Web Application Tests policy
- Input target IP: 192.168.56.104
- Click Save and Launch

### Step 3 — Analyze Results
- Navigate to Vulnerabilities tab
- Review findings by severity

## Scan Results

### Summary
| Metric | Value |
|---|---|
| Host scanned | 192.168.56.104 |
| Total vulnerabilities | 28 |
| Scan policy | Web Application Tests |
| Status | Completed |

### Key Vulnerabilities Found

| Severity | CVSS | Vulnerability | Family |
|---|---|---|---|
| INFO | 7.5 | CGI Generic Remote Code Execution | CGI Abuses |
| INFO | 5.3 | Browsable Web Directories | CGI Abuses |
| INFO | 5.0 | Backup Files Disclosure | CGI Abuses |
| INFO | 4.3 | CGI Generic Cookie Injection | CGI Abuses |
| INFO | 4.3 | CGI Generic HTML Injection | CGI Abuses: XSS |
| INFO | 4.3 | CGI Generic XSS | CGI Abuses: XSS |

## Key Observations
- Nessus automatically discovered 28 vulnerabilities
- Multiple XSS and injection vulnerabilities found
- Browsable directories expose sensitive files
- Backup file disclosure could leak source code
- CGI Remote Code Execution is highest risk finding

## Defensive Countermeasures

| Vulnerability | Fix |
|---|---|
| Remote Code Execution | Patch CGI scripts, disable unused CGI |
| Browsable Directories | Disable directory listing in Apache |
| Backup File Disclosure | Remove backup files from web root |
| Cookie Injection | Validate and sanitize all cookie inputs |
| XSS | Implement input validation and CSP headers |

## Key Takeaway
Nessus automates vulnerability discovery across
all services simultaneously — what would take
hours manually is completed in minutes.
Understanding how to interpret CVSS scores and
prioritize findings is an essential SOC Analyst
skill.
