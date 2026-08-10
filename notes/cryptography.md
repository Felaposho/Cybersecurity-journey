# Cryptography Fundamentals Lab

## Objective
Demonstrate the three core cryptographic concepts:
encoding, hashing, and encryption using 
command line tools on Kali Linux

## Tool Used
- Kali Linux terminal
- base64 — encoding/decoding
- sha256sum — hashing
- openssl — encryption/decryption

---

## Part 1 — Base64 Encoding & Decoding

### Encoding
```bash
echo -n 'This is confidential' | base64
```
**Output:**
```
VGhpcyBpcyBjb25maWRlbnRpYWw=
```

### Decoding
```bash
echo -n 'VGhpcyBpcyBjb25maWRlbnRpYWw=' | base64 -d
```
**Output:**
```
This is confidential
```

### Key Observation
Base64 is ENCODING not ENCRYPTION
Anyone can decode it without a key
Used for data transfer not security

---

## Part 2 — SHA256 Hashing

### Hash 1
```bash
echo -n 'Hello World' | sha256sum
```
**Output:**
```
a591a6d40bf420404a011733cfb7b190d62c65bf0bcda32b57b277d9ad9f146e
```

### Hash 2
```bash
echo -n 'Hello world' | sha256sum
```
**Output:**
```
64ec88ca00b268e5ba1a35678a1b5316d212f4f366b2477232534a8aeca37f3c
```

### Key Observation — Avalanche Effect
Changing ONE character (W → w) produces a 
completely different hash — this is the 
Avalanche Effect in cryptography

SHA256 properties:
- One way — cannot be reversed
- Fixed output length regardless of input size
- Same input always produces same output
- Used for password storage and file integrity

---

## Part 3 — AES-256-CBC File Encryption

### Encrypt file
```bash
openssl enc -aes-256-cbc -in /etc/hostname \
-out encrypted_hostname.bin \
-k mysecretpassword -pbkdf2
```

### Decrypt file
```bash
openssl enc -d -aes-256-cbc \
-in encrypted_hostname.bin \
-out decrypted_hostname.txt \
-k mysecretpassword -pbkdf2
```

### Verify decryption
```bash
cat decrypted_hostname.txt
```
**Output:**
```
kali
```

### Key Observation
AES-256-CBC successfully encrypted and 
decrypted the file — original content recovered!

Parameters explained:
- `-aes-256-cbc` — algorithm used
- `-in` — input file
- `-out` — output file
- `-k` — encryption key
- `-pbkdf2` — key derivation function

---

## Comparison Table

| Type | Example | Reversible | Key Required | Use Case |
|---|---|---|---|---|
| Encoding | Base64 | ✅ Yes | ❌ No | Data transfer |
| Hashing | SHA256 | ❌ No | ❌ No | Integrity check |
| Encryption | AES-256 | ✅ Yes | ✅ Yes | Data protection |

## Key Takeaway
Cryptography is the backbone of cybersecurity.
Understanding the difference between encoding,
hashing and encryption is essential for any
security professional — each serves a completely
different purpose and choosing the wrong one
can leave data exposed.
