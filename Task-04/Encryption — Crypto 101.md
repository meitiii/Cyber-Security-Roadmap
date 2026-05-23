# 🔐 Encryption — Crypto 101 Runbook

This room provides an essential introduction to the pillars of modern cryptography: Symmetric/Asymmetric encryption, RSA mechanics, digital signatures, and practical GPG/SSH key handling.

---

## 🔑 Core Concepts
* **Symmetric Encryption:** Uses the same key for encryption/decryption (e.g., **AES**). Fast, efficient, suitable for bulk data.
* **Asymmetric Encryption:** Uses a key pair (**Public/Private**). The Public key is shared; the Private key is secret. (e.g., **RSA**, **ECC**).
* **Digital Signatures:** Used to verify the **authenticity** and **integrity** of data using asymmetric keys.
* **Certificates:** Digital "IDs" used by web servers (HTTPS) to prove identity, validated by a Certificate Authority (CA).
* **Modulus Operator (`%`):** Returns the remainder of division. Crucial in many cryptographic mathematical challenges (e.g., $30 \% 5 = 0$).

---

## 🛠️ Practical Utilities & Workflows
* **SSH Key Cracking:**
    1. Extract hash: `ssh2john id_rsa > ssh.hash`
    2. Crack with JtR: `john --wordlist=rockyou.txt ssh.hash`
* **GPG Decryption:**
    1. Import private key: `gpg --import private.key`
    2. Decrypt file: `gpg --decrypt file.gpg > output.txt`
* **RSA Math:** $n = p \times q$. Understanding these variables is critical for CTF challenges involving broken RSA.

---

## 🛡️ Remediation & Best Practices
* **Algorithm Trust:** **Never** trust `DES` or `Triple DES` for new implementations. Use `AES` (symmetric) or modern asymmetric standards.
* **Public Key Hygiene:** It is safe (and necessary) to share public keys; never share private keys or compromise their passphrase.
* **Defense in Depth:** Use `PCI-DSS` standards if processing payment data, and rely on standard protocols like `SSH` (Secure Shell) for remote access rather than unencrypted alternatives.

---

## 🧠 Key Takeaways
* **Confidentiality, Integrity, Authenticity:** Cryptography is the foundation of these three pillars.
* **The "Box" Metaphor:** Asymmetric crypto is often used to securely exchange symmetric keys. You send the "lock" (Public Key), and the recipient sends data back in an "indestructible box" (Symmetric Key), allowing you to communicate efficiently.
* **Quantum Threat:** Future quantum computing capabilities pose a significant risk to current RSA-based encryption because they can solve the prime factorization problems underlying RSA very efficiently.
* **PGP/GPG:** These are powerful tools for file encryption and signing. Like SSH keys, their private components should always be protected by strong passphrases and treated as highly sensitive.

---
**Task Status:** ✅ Completed