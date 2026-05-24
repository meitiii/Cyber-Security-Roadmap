# 🛡️ Cryptography Basics — Runbook

## 📝 Executive Summary
This room introduces foundational cryptographic concepts, covering the distinction between plaintext and ciphertext, historical ciphers like Caesar, and core symmetric encryption standards. It emphasizes the necessity of modern, secure algorithms over obsolete ones like DES.

---

## 🔑 Core Concepts
* **Plaintext:** Data in its original, readable format.
* **Ciphertext:** Data that has been encrypted and is unreadable without the correct key.
* **Decryption:** The process of converting ciphertext back into plaintext.
* **Caesar Cipher:** A historical substitution cipher where each letter is shifted a certain number of positions down the alphabet.
* **XOR (⊕):** A fundamental logical operation used extensively in modern cryptography (especially in symmetric ciphers).

---

## 🛠️ Practical Reference
* **Modulo Operator (%):** Returns the remainder after division.
  * *Example:* `60 % 12 = 0`
* **XOR Operation:**
  * `1001 ⊕ 1010` = `0011`
* **Encryption Standards:**
  * **DES (Data Encryption Standard):** Insecure/Broken. Do not use.
  * **AES (Advanced Encryption Standard):** Adopted in **2001** as the secure successor to DES.

---

## 🛡️ Remediation & Best Practices
* **Regulatory Compliance:** Always adhere to standards like **PCI DSS** (Payment Card Industry Data Security Standard) when handling sensitive financial data.
* **Algorithm Selection:** Never implement custom or outdated encryption (like Caesar or DES). Use industry-verified symmetric standards like **AES-256**.
* **Integrity & Secrecy:** Distinguish between *encoding* (representation), *hashing* (integrity), and *encryption* (secrecy). Use the correct tool for each requirement.

---

## 🧠 Key Takeaways
* **Historical vs. Modern:** Historical ciphers (e.g., Caesar) are useful for teaching logic but provide zero security against modern cryptanalysis.
* **Mathematics Foundation:** Cryptography relies heavily on discrete math, specifically modular arithmetic and bitwise XOR operations. Understanding these is essential for breaking or building secure systems.
* **Evolution of Standards:** Cryptography is a cat-and-mouse game; as computing power increases, older standards like DES become vulnerable and must be replaced by robust alternatives like AES.

---
**Task Status:** ✅ Completed