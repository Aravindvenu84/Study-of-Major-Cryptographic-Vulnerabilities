# Study-of-Major-Cryptographic-Vulnerabilities
This section covers historically significant cryptographic vulnerabilities that exposed weaknesses in SSL/TLS implementations. Each vulnerability is explained with its root cause and a potential exploit scenario.

## 1. Heartbleed (OpenSSL Bug)

Type: Information disclosure
Affected: OpenSSL (2012–2014)

### 🔍 Root Cause

Heartbleed was caused by a missing bounds check in the TLS Heartbeat extension.
The server trusted the client-provided payload length without verifying its actual size, leading to out-of-bounds memory reads.

### ⚠️ Exploit Scenario

An attacker sends a malformed heartbeat request claiming a large payload size.
The server responds by leaking up to 64 KB of its memory, which may contain:

Private keys

User credentials

Session cookies

This attack is silent and leaves no logs.

## 2. BEAST Attack (Browser Exploit Against SSL/TLS)

Type: Chosen-plaintext attack
Affected: TLS 1.0 (CBC mode)

### 🔍 Root Cause

BEAST exploits the predictable Initialization Vector (IV) used in CBC mode encryption in TLS 1.0.
Because the IV for each block is derived from the previous ciphertext, attackers can infer plaintext values.

### ⚠️ Exploit Scenario

An attacker positioned as a Man-in-the-Middle (MITM) injects JavaScript into a victim’s browser and observes encrypted traffic.
By carefully crafting requests, the attacker can decrypt secure cookies byte-by-byte, leading to session hijacking.

## 3. DROWN Attack (Decrypting RSA with Obsolete and Weakened eNcryption)

Type: Cross-protocol attack
Affected: SSLv2 + TLS servers sharing the same RSA key

### 🔍 Root Cause

DROWN occurs when a server still supports SSLv2, a deprecated and insecure protocol, and uses the same RSA private key for modern TLS.

### ⚠️ Exploit Scenario

An attacker sends crafted SSLv2 messages to the vulnerable server.
By exploiting SSLv2 weaknesses, the attacker can decrypt TLS traffic, even though TLS itself is secure.

This allows:

Decryption of past sessions

Compromise of HTTPS connections

## 4. CRIME Attack (Compression Ratio Info-leak Made Easy)

Type: Side-channel attack
Affected: TLS compression + HTTP compression

### 🔍 Root Cause

CRIME exploits data compression before encryption.
When secret data (like cookies) and attacker-controlled input are compressed together, the attacker can infer secrets based on compressed size differences.

### ⚠️ Exploit Scenario

An attacker performs a MITM attack and injects chosen plaintext into requests.
By observing the size of compressed encrypted traffic, the attacker can guess secret session cookies, leading to account takeover.

## 🛡️ Security Lessons Learned

Legacy protocols (SSLv2, TLS 1.0) must be completely disabled

Encryption implementations must include strict input validation

Compression and encryption should be carefully combined

Backward compatibility can introduce severe vulnerabilities
