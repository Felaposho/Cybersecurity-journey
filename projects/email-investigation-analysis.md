# Email Threat Investigation Analysis

## Overview
Investigation of 13 emails to determine whether 
each was Safe, Suspicious, or Malicious

## Organization
Skillboost Africa

## Analysis Framework
Each email was analyzed against:
- SPF/DKIM/DMARC authentication headers
- Sender IP and infrastructure
- Message content and social engineering indicators
- Embedded URLs and attachments
- Reply-To and Return-path consistency

---

## Results Summary

| Email | Sender | Verdict | Threat Type | Key IOC |
|---|---|---|---|---|
| 1 | IT Helpdesk – Solvex | ✅ SAFE | None | None |
| 2 | IT Support Desk | ⚠️ SUSPICIOUS | Phishing | Malicious URL |
| 3 | Ramesh Kulkarni – Apex Steel | ⚠️ SUSPICIOUS | BEC Payment Diversion | Reply-To mismatch |
| 4 | HR – Solvex Industries | ✅ SAFE | None | None |
| 5 | Rajeev Malhotra (MD) | ⚠️ SUSPICIOUS | BEC Impersonation | Social Engineering |
| 6 | Google Drive | ✅ SAFE | None | None |
| 7 | Microsoft account team | ⚠️ SUSPICIOUS | Phishing | Typosquat domain |
| 8 | Neha Kapoor HR | 🔴 MALICIOUS | Malware Delivery | Malicious ZIP file |
| 9 | Tally Solutions Billing | ✅ SAFE | None | None |
| 10 | International Lottery Board | ⚠️ SUSPICIOUS | Advance-fee Scam | Reply-To mismatch |
| 11 | Suresh Iyer (CFO) | ⚠️ SUSPICIOUS | BEC CFO Fraud | Reply-To mismatch |
| 12 | InfoSec – Solvex Industries | ✅ SAFE | None | None |
| 13 | Accounts Receivable | 🔴 MALICIOUS | Macro Malware | .docm attachment |

---

## Overall Statistics
| Verdict | Count |
|---|---|
| ✅ Safe | 5 |
| ⚠️ Suspicious | 6 |
| 🔴 Malicious | 2 |

---

## Key Findings

### 1. Typosquatting Detection (Email 7)
Domain: micros0ft-online.com
Used zero (0) instead of letter O to impersonate Microsoft
Combined with SPF/DKIM/DMARC failure and unknown IP

### 2. BEC CFO Fraud (Email 11)
SPF/DKIM/DMARC all passed on visible From address
However Reply-To routed to external outlook.com address
Classic CFO fraud — passed authentication checks
but Reply-To mismatch revealed the attack

### 3. Macro-based Malware (Email 13)
Attached: Pending_Invoice_Challan_details.docm
Requested recipient to enable macros
Classic technique to execute malicious VBA code
SPF/DKIM/DMARC all failed
Domain not authorized to send

### 4. Payment Diversion BEC (Email 3)
SPF/DKIM/DMARC all passed
However Reply-To pointed to different domain
Attempted to redirect vendor payments to attacker account

---

## Common Attack Patterns Observed
Social engineering was the dominant pattern across
all flagged emails using:
- **Urgency** — "IMMEDIATELY", "within 24 hours"
- **Secrecy** — "Don't discuss with anyone"
- **Authority impersonation** — MD, CFO, Microsoft
- **Requested action** — click link, enable macros, wire money
- **Fear-based consequences** — "permanent loss of access"

---

## Recommendations
1. Enroll recipient in phishing/BEC security awareness training
2. Verify payment/wire-transfer requests through independent channel
3. Block identified malicious domains at email gateway
4. Quarantine malicious emails organization-wide
5. Never enable macros on documents from unknown senders
6. Always verify Reply-To matches From address before responding

---

## Key Takeaway
Authentication headers (SPF/DKIM/DMARC) are important
but not sufficient alone — Email 11 passed all three
yet was still a BEC attack. Human analysis of
Reply-To mismatches, social engineering indicators,
and content context remains essential for accurate
email threat investigation.
