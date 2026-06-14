# OSINT Reconnaissance with SpiderFoot

## Tool
SpiderFoot v4.0 — open source OSINT automation tool
pre-installed on Kali Linux

## Target
- scanme.nmap.org
- Officially sanctioned practice target maintained 
by the Nmap team for security research

## Objective
To gather publicly available intelligence about a 
target domain and understand what attackers can see
before launching an attack

## Methodology
1. Launched SpiderFoot on Kali Linux
2. Created new scan with target: scanme.nmap.org
3. Selected Footprint scan type
4. Analysed results from Summary, Browse and Graph

## Key Findings
| Type | Finding |
|---|---|
| IP Address | 45.33.32.156 |
| IPv6 Address | 2600:3c01::f03c:91ff:fe18:bb2f |
| DNS Record | ns2.linode.com (Linode hosting) |
| Open Port 1 | 45.33.32.156:22 (SSH) |
| Open Port 2 | 45.33.32.156:80 (HTTP) |
| Web Technology | PHP |
| Linked URLs | Facebook and multiple external sites |

## Scan Statistics
- Total Results: 470
- Unique Findings: 344
- Errors: 134 (normal — API keys not configured)

## Analysis
- Server is hosted on Linode cloud infrastructure
- Port 22 (SSH) is open — potential brute force risk
- Port 80 (HTTP) running without HTTPS — 
  unencrypted traffic risk
- PHP detected — known vulnerabilities if outdated
- Multiple external URL connections identified

## What I Learned
- How to use SpiderFoot for OSINT reconnaissance
- How to identify exposed server infrastructure
- How open ports reveal potential attack surfaces
- Why minimizing digital footprint matters

## Key Takeaway
OSINT reconnaissance reveals what attackers can 
see before launching an attack. Even a simple 
footprint scan exposed the server IP, hosting 
provider, open ports, and web technologies — 
all valuable information for both attackers and 
defenders.

## Tools Used
- SpiderFoot v4.0
- Kali Linux VM
- VirtualBox
