# Firewall Configuration with GUFW

## Objective
Configure a host-based firewall using GUFW 
(Graphical Uncomplicated Firewall) on Kali Linux
and create custom rules to control network traffic

## Tools Used
- Kali Linux
- GUFW (Graphical Uncomplicated Firewall)

## Lab Environment
| Machine | IP | Role |
|---|---|---|
| Kali Linux | 192.168.56.102 | Host being protected |
| Metasploitable 2 | 192.168.56.104 | Target/Remote host |

---

## Step 1 — Firewall Profile Configuration

| Setting | Value | Reason |
|---|---|---|
| Profile | Home | Local network environment |
| Status | Enabled | Firewall active |
| Incoming | Deny | Block all unsolicited inbound traffic |
| Outgoing | Allow | Permit outbound connections |

**Key observation:**
Setting Incoming to Deny by default follows the
principle of least privilege — nothing gets in
unless explicitly allowed

---

## Step 2 — Creating Advanced Firewall Rule

### Rule Configuration
| Parameter | Value |
|---|---|
| Name | practice |
| Insert | 0 (highest priority) |
| Policy | Deny |
| Direction | Out (Outbound) |
| Interface | eth1 |
| Log | Enabled |
| Protocol | TCP |
| From IP | 192.168.56.102 |
| From Port | 443 |
| To IP | 192.168.56.104 |
| To Port | 443 |

### What this rule does
Blocks outbound TCP traffic from Kali Linux
(192.168.56.102) to Metasploitable 2
(192.168.56.104) on port 443 (HTTPS)
and logs all attempts

---

## Firewall Concepts Demonstrated

| Concept | Explanation |
|---|---|
| Default Deny | Block everything unless explicitly allowed |
| Rule Priority | Insert 0 = highest priority rule |
| Direction | Inbound vs Outbound traffic control |
| Logging | Record all matching traffic for audit |
| Protocol filtering | TCP/UDP/Both can be specified |

## Key Takeaways
1. Firewalls are the first line of defence
2. Default deny inbound = most secure posture
3. Rules are processed in order — priority matters
4. Logging is essential for security monitoring
5. Host-based firewalls protect individual machines
   while network firewalls protect entire networks
