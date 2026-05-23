# 🔐 Hashing Basics — Runbook

## 📝 Executive Summary
This room provides a practical follow-up to fundamental hashing concepts, focusing on file integrity verification, hands-on cracking techniques using Hashcat, and the distinction between cryptographic hashing, encoding, and HMAC (Keyed-Hash Message Authentication Code).

---

## 🌐 Core Concepts
* **Hashing:** A one-way function converting input into a fixed-length, unique "digest." Even a one-bit change in input drastically changes the output.
* **Integrity:** Using hashes to ensure files have not been tampered with or corrupted during download.
* **HMAC:** Combines a hash function with a secret key to ensure both **integrity** (no changes) and **authenticity** (sender identity).
* **Encoding vs. Encryption:** Encoding (e.g., Base64) is for data compatibility and is reversible without a key. Encryption is for confidentiality and requires a key to reverse.

---

## 🛠️ Practical Reference

### File Integrity & Decoding
* **SHA256 Hash Calculation:** `sha256sum <file>`
* **Base64 Decoding:** `echo "<encoded_string>" | base64 -d`

### Hashcat Operations
* **Command Pattern:** `hashcat -m <mode> -a 0 <hash_file> <wordlist>`
* **Cisco-ASA MD5:** Mode `2410`
* **HMAC-SHA512:** Mode `1750`

### Hash Identification Table
| Algorithm | Identifier/Context |
| :--- | :--- |
| **MD5** | 32-character hex string (Obsolete) |
| **yescrypt** | Modern Linux; 256-bit size |
| **Cisco-IOS ($9$)** | Uses `scrypt` |

---

## 🛡️ Remediation & Best Practices
* **Never store passwords in plaintext:** Always use robust, salted hashing algorithms (e.g., bcrypt, yescrypt, scrypt).
* **Verify Official Downloads:** Always compute and compare the hash (MD5/SHA256) of downloaded software packages against the provider's official checksum to prevent supply-chain attacks.
* **Avoid Obsolete Hashes:** MD5 and SHA1 are computationally broken regarding collision resistance and should never be used for security-critical applications.

---

## 🧠 Key Takeaways
* **Don't Confuse Roles:** Hashing is for verification (integrity), while encryption is for secrecy (confidentiality).
* **HMAC is Powerful:** By adding a secret key, HMAC protects against attackers who might otherwise modify a hash to match tampered data.
* **Tooling:** Always refer to official documentation (like `hashcat.net/wiki`) to identify the correct mode number; manual identification via character count/prefix is a starting point, but mode numbers are definitive for automated tools.

---
**Task Status:** ✅ Completed