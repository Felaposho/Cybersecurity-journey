# MAC Address Spoofing with Macchanger

## What is MAC Address Spoofing?
Changing a device's MAC address to hide its true 
identity on a network or bypass access controls.

## Why it matters in Cybersecurity
- Attackers use it to bypass network filters
- Helps maintain anonymity on a local network
- Defenders must detect unauthorized MAC changes
- Used in wireless network attacks

## Tools Used
- Kali Linux
- Macchanger
- macaddress.io (verification)

## Commands Used

### View all network interfaces
```bash
ip a
```

### Check current MAC address
```bash
macchanger --show eth0
```

### Bring interface down
```bash
sudo ip link set eth0 down
```

### Spoof MAC randomly
```bash
sudo macchanger -r eth0
```

### Bring interface back up
```bash
sudo ip link set eth0 up
```

### Verify new MAC is active
```bash
macchanger --show eth0
```

### Restore original MAC
```bash
sudo macchanger -p eth0
```

## Results
| | MAC Address | Vendor |
|---|---|---|
| **Original** | 08:00:27:8a:35:d2 | CADMUS COMPUTER SYSTEMS |
| **Spoofed** | 56:8b:2e:f4:4a:85 | Unknown |

## Verification
Verified spoofed MAC on macaddress.io:
- Valid: True
- Administration type: LAA (Locally Administered Address)
- Vendor: None (confirms successful spoof) ✅

## Key Takeaway
MAC spoofing is a fundamental network anonymity 
technique. Understanding it from an attacker's 
perspective helps defenders implement better 
network monitoring and access controls.
