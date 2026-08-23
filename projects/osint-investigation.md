# OSINT Investigation & Intelligence Gathering Report

## Target
- **URL:** https://www.chicken-road2.app/
- **Description:** Online gaming/betting platform
- **Organization:** Skillboost Africa

## Tools Used
| Tool | Purpose |
|---|---|
| WHOIS | Domain registration info |
| dig | DNS resolution |
| dnsenum | DNS enumeration |
| whois (IP) | Hosting provider identification |
| openssl | SSL certificate analysis |
| curl | Security headers inspection |
| Wappalyzer | Technology stack fingerprinting |
| subfinder | Subdomain enumeration |
| Browser | robots.txt and sitemap.xml review |

---

## Findings Summary

### 3.1 Domain Information
| Property | Value |
|---|---|
| Registrar | Key-Systems LLC |
| Created | 13 May 2025 |
| Updated | 4 May 2026 |
| Expires | 13 May 2027 |
| Status | Client Transfer Prohibited |

### 3.2 WHOIS Details
| Property | Value |
|---|---|
| Registrar | Key-Systems |
| Name Servers | autumn.ns.cloudflare.com, denver.ns.cloudflare.com |
| Status | Client Transfer Prohibited |

### 3.3 IP Addresses
```bash
dig chicken-road2.app
```
| Record | TTL | Type | IP |
|---|---|---|---|
| chicken-road2.app | 214 | A | 104.21.38.193 |
| chicken-road2.app | 214 | A | 172.67.137.239 |

Both IPs belong to Cloudflare — true origin masked

### 3.4 DNS Records
```bash
dnsenum chicken-road2.app
```
- Host Addresses: 104.21.38.193, 172.67.137.239
- NS Records: Cloudflare
- MX Records: Not configured (no mail service)

### 3.5 Hosting Provider
```bash
whois 104.21.38.193
```
- Provider: Cloudflare Inc. (CLOUD14)
- True origin server undetermined — proxy masking active

### 3.6 SSL Certificate
```bash
openssl s_client -connect chicken-road2.app:443 \
</dev/null 2>/dev/null | openssl x509 -noout -text
```
| Property | Value |
|---|---|
| Issuer | Google Trust Services |
| Algorithm | SHA256 with ECDSA |
| Valid From | July 5 2026 |
| Valid Until | October 3 2026 |
| Key | 256-bit EC Public Key |

### 3.7 Security Headers
```bash
curl -I https://www.chicken-road2.app/
```
| Header | Status |
|---|---|
| Content-Security-Policy | ❌ Missing |
| X-Frame-Options | ❌ Missing |
| X-Content-Type-Options | ✅ Present |
| Strict-Transport-Security | ✅ Present |
| Server | Cloudflare |
| Powered-By | Next.js |

### 3.8 Technology Stack
Identified via Wappalyzer:
| Category | Technology |
|---|---|
| JavaScript Framework | React |
| Web Framework | Next.js 16.2.12 |
| PaaS | Vercel |
| UI Framework | Tailwind CSS |
| Video Player | VideoJS |
| Protocol | HTTP/3 |

### 3.9 Subdomains
```bash
subfinder -d www.chicken-road2.app
```
Result: 0 subdomains found in 377 milliseconds

### 3.10 robots.txt
| User-Agent | Rule |
|---|---|
| * | Disallow: /api/*, /go/*, /pages/condition-utilisation |
| AhrefsBot | Disallow: * |
| SemrushBot | Disallow: / |
| dotbot | Disallow: * |
| Screaming Frog | Disallow: * |
| ChatGPT-User | Disallow: / |

⚠️ Internal paths /api/ and /go/ exposed publicly!

### 3.11 Sitemap.xml
- Result: Empty — no content indexed

---

## Security Observations

### Observation 1 — Missing Security Headers 🔴
**Finding:** Content-Security-Policy and X-Frame-Options missing
**Impact:** Site vulnerable to XSS and clickjacking attacks
**Fix:** Implement CSP and X-Frame-Options headers

### Observation 2 — robots.txt Path Disclosure 🟡
**Finding:** Internal paths /api/*, /go/* exposed in robots.txt
**Impact:** Hands sensitive path structure to attackers doing recon
**Fix:** Remove sensitive paths from robots.txt — restrict at auth layer

### Observation 3 — Cloudflare Protection ✅
**Finding:** True origin IP masked behind Cloudflare proxy
**Impact:** Reduces DDoS risk and direct-to-origin attacks
**Note:** This is a positive security practice

---

## Recommendations
1. Implement Content-Security-Policy header to prevent XSS
2. Add X-Frame-Options header to prevent clickjacking
3. Remove sensitive paths from robots.txt
4. Continue leveraging Cloudflare reverse proxy
5. Periodically recheck SSL/TLS via SSL Labs
6. Populate sitemap.xml if SEO visibility is desired

---

## Key Takeaway
OSINT reveals what attackers see before they touch
a target. Even a well-protected site behind Cloudflare
can leak internal structure through robots.txt.
The combination of missing security headers and
exposed API paths creates an attack surface that
could be exploited despite the strong infrastructure.
