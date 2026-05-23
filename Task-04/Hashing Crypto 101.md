# 🔐 Hashing Crypto 101 — Runbook

This room covers the fundamentals of hashing, the difference between encoding and encryption, password cracking methodologies, and data integrity verification.

---

## 📚 Concepts & Definitions
* **Encoding:** Not encryption. A data representation format (e.g., Base64, Hex). Immediately reversible without a key.
* **Hashing:** A one-way function that produces a fixed-size "digest" from input data. Intended to be impossible to reverse.
* **Hash Collision:** When two different inputs produce the same hash output (avoidable in theory, but technically possible in insecure algorithms like MD5/SHA1).
* **Salt:** Random data added to a password before hashing. Prevents the use of precomputed **Rainbow Tables** (lookup tables of pre-calculated hashes).

---

## 🔨 Password Cracking Methodology
Since hashes cannot be decrypted, they must be "cracked" via brute force or dictionary attacks.

1. **Identification:** Use tools like `hashID` or analyze prefixes (e.g., `$6$` for `sha512crypt`).
2. **Dictionary Attack:** Compare target hashes against a list of pre-hashed common passwords (e.g., `rockyou.txt`).
3. **GPU Acceleration:** Use tools like `Hashcat` or `John the Ripper`. GPU cores are significantly faster at calculating hashes than CPUs.
4. **Safety:** Never use the `--force` flag in Hashcat to avoid false positives/negatives.

---

## ⌨️ Command & Tool Reference
* **Hashcat:**
    * `hashcat -m <mode> -a 0 <hash_file> <wordlist>`
    * *Example (Bcrypt mode 3200):* `hashcat -m 3200 hash.txt rockyou.txt`
* **Hash Modes (Key Examples):**
    * `3200`: Bcrypt
    * `1750`: HMAC-SHA512
* **Integrity Checking:**
    * `sha1sum <file>`: Verify file integrity by comparing the output hash against the provider's known good hash.

---

## 🛡️ Remediation (Defense)
* **Never use MD5/SHA1 for passwords:** They are computationally too fast and insecure against collision attacks.
* **Use Modern Hashing:** Use `bcrypt`, `argon2`, or `scrypt`. These are intentionally slow (CPU/Memory intensive), making brute-force attacks significantly harder.
* **Always Salt:** Ensure every user has a unique salt, even if they share the same password.
* **Store Securely:** Never store passwords in plaintext or use reversible encryption for storage.

---

## 🧠 Key Takeaways
* **Integrity vs. Secrecy:** Hashing is for verifying that data hasn't changed (integrity); Encryption is for keeping data secret (confidentiality).
* **Research is Crucial:** Different applications use unique hashing configurations; always check documentation or official wiki pages (like `hashcat.net/wiki`) to identify the correct mode.
* **Pigeonhole Principle:** Collisions are mathematically inevitable in hashing because the input space is infinite while the output space is fixed.

---
**Task Status:** ✅ Completed