# Denial of Service (DoS) Attack Lab

## Objective
Demonstrate how DoS attacks overwhelm a web server
and make it unavailable to legitimate users

## Lab Environment
| Machine | IP | Role |
|---|---|---|
| Kali Linux | 192.168.56.102 | Attacker |
| Metasploitable 2 | 192.168.56.104 | Target |

## Tools Used
- `curl` — HTTP header inspection
- `ApacheBench (ab)` — HTTP flood tool
- `Slowloris` — slow HTTP DoS tool
- `Wireshark` — traffic analysis

---

## Step 1 — Reconnaissance
```bash
curl -I 192.168.56.104
```
**Findings:**
- Server: Apache/2.2.8 (Ubuntu) DAV/2
- PHP version: 5.2.4-2ubuntu5.10
- Port: 80
- Response: HTTP/1.1 200 OK

---

## Step 2 — HTTP Flood with ApacheBench
```bash
ab -n 10000 -c 100 http://192.168.56.104/
```

**Parameters:**
- `-n 10000` — total requests to send
- `-c 100` — concurrent connections

**Results:**
| Metric | Value |
|---|---|
| Total requests | 10,000 |
| Failed requests | 0 |
| Time taken | 276.371 seconds |
| Requests/second | 36.18 |
| Total transferred | 10,719,510 bytes |
| Longest request | 6,814ms |

**Wireshark observation:**
Massive SYN packets flooding port 80 visible
in packet capture — TCP handshakes overwhelming
the server connection table

---

## Step 3 — Slowloris Attack
```bash
slowloris 192.168.56.104 -s 500 -p 80
```

**Parameters:**
- `-s 500` — open 500 simultaneous sockets
- `-p 80` — target port 80

**What Slowloris does:**
Opens hundreds of connections to the web server
and keeps them alive by sending partial HTTP
headers slowly — never completing the request.
This exhausts the server's connection pool.

**Results:**
- Socket count gradually increased to 293+
- Website became completely unreachable during attack
- Server recovered immediately after attack stopped

---

## Noticeable Vulnerabilities

### 1. Slowloris Vulnerability
Apache/2.2.8 has no connection timeout configured
→ Connections stay open indefinitely
→ Server connection pool gets exhausted

### 2. No Rate Limiting
Server accepts unlimited concurrent connections
→ Easy to overwhelm with ApacheBench flood

### 3. Outdated Software
Apache 2.2.8 and PHP 5.2.4 are severely outdated
→ Many known vulnerabilities exist

---

## Defensive Countermeasures

| Attack | Defence |
|---|---|
| Slowloris | Enable mod_reqtimeout on Apache |
| HTTP Flood | Implement rate limiting |
| Both | Deploy WAF (Web Application Firewall) |
| Both | Use Cloudflare DDoS protection |
| General | Upgrade Apache and PHP versions |

## Key Takeaway
DoS attacks don't always require massive bandwidth —
Slowloris proved a single machine with 500 sockets
can take down an unprotected Apache server completely.
Understanding attack techniques helps defenders
build more resilient systems.
