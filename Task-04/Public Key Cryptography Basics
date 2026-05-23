# 🛡️ Public Key Cryptography Basics — Runbook

## 📝 Executive Summary
This room explores the practical applications of asymmetric (public-key) cryptography. It covers how RSA and Diffie-Hellman facilitate secure key exchanges, how SSH and GPG secure communications, and the role of Certificates and Digital Signatures in verifying identity and data integrity on the internet.

---

## 🔑 Core Concepts
* **Symmetric vs. Asymmetric:** Symmetric uses one key (fast/bulk data); Asymmetric uses a key pair (Public/Private) to secure the initial exchange and verify identity.
* **RSA:** An algorithm based on the difficulty of factoring large numbers ($n = p \times q$).
* **Diffie-Hellman (DH):** A method for two parties to establish a shared secret over an insecure channel without prior knowledge.
* **Digital Signatures:** Prove **Authenticity** and **Integrity** by signing data with a private key.
* **Certificates:** Electronic documents (e.g., HTTPS certificates) validated by a Certificate Authority (CA) to prove a server's identity.

---

## 🛠️ Practical Reference
### RSA Math
* **Modulus ($n$):** $n = p \times q$
* **Euler’s Totient ($\phi(n)$):** $\phi(n) = (p-1) \times (q-1)$

### Diffie-Hellman Key Exchange (for $A = g^a \pmod p$)
* **Shared Key Calculation:** If Alice has private key $a$ and Bob has public key $B$, the shared key is $B^a \pmod p$. (This will equal $A^b \pmod p$ for Bob).

### SSH & GPG Operations
* **SSH Key Gen:** `ssh-keygen -t <algorithm>`
* **SSH Login:** `ssh -i <private_key> user@host` (Permissions must be `600`).
* **GPG Import/Decrypt:**
    * Import: `gpg --import <key_file>`
    * Decrypt: `gpg --decrypt <encrypted_file>`

---

## 🛡️ Remediation & Best Practices
* **Identity Verification:** Always rely on valid TLS certificates for web services. Check for valid chains of trust rather than just the presence of HTTPS.
* **Security Standards:** Use modern SSH key algorithms like `Ed25519` over older `DSA` or `RSA` when possible.
* **Key Management:** Private keys (SSH/GPG) should always be protected by strong passphrases and kept in secure, backed-up locations.
* **Permissions:** For SSH, the private key must be readable only by the owner (`chmod 600`) to prevent unauthorized access.

---

## 🧠 Key Takeaways
* **The "Lock & Key" Metaphor:** Asymmetric crypto is the "lock" that allows for the safe transport of the "symmetric key" (the actual data cipher), combining the security of asymmetric methods with the speed of symmetric ones.
* **Authenticity is Paramount:** Encryption alone provides confidentiality, but without Digital Signatures or Certificates, you cannot guarantee you are talking to the intended party.
* **CTF Utility:** Many RSA-based CTF challenges rely on manipulating $p, q, n, e, d$. Mastering these variables is essential for breaking such puzzles.

---
**Task Status:** ✅ Completed