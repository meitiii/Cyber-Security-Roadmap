# 🔐 Cryptography Basics — TryHackMe (Summary)

This room introduces the fundamental concepts of cryptography, covering the historical evolution of ciphers, the shift toward modern symmetric encryption standards, and the basic mathematical operations that underpin these technologies.

---

## 🌐 Core Cryptographic Concepts

- **Plaintext:** The original, readable message.
- **Ciphertext:** The encrypted, unreadable result.
- **Encryption:** The process of converting plaintext into ciphertext.
- **Decryption:** The process of reversing encryption to retrieve the original plaintext.

---

## 📜 Historical & Modern Standards

### Historical Ciphers
- **Caesar Cipher:** A simple substitution cipher where each letter in the plaintext is shifted a certain number of positions down the alphabet. 
  - *Example:* `XRPCTCRGNEI` (shifted) becomes `ICANENCRYPT`.

### Modern Standards
- **DES (Data Encryption Standard):** An outdated and insecure encryption standard that should no longer be used.
- **AES (Advanced Encryption Standard):** The current global encryption standard, adopted in **2001** for secure data protection.
- **Compliance:** **PCI DSS** (Payment Card Industry Data Security Standard) is the mandatory security standard for organizations that handle credit card information.

---

## 🧮 Basic Cryptographic Math

Modern cryptography relies heavily on mathematical operations:

- **XOR (⊕):** A logical operation where two bits are compared; if bits are different, the output is 1, if same, the output is 0.
  - *Example:* $1001 \oplus 1010 = 0011$.
- **Modulo (%):** The remainder of a division operation.
  - *Example:* $60 \pmod{12} = 0$, $118613842 \pmod{9091} = 3565$.

---

## 🧠 Career Direction Insight

Understanding how data is encrypted and the math behind it is essential for roles involving secure communication, data privacy, and vulnerability assessment.

- **Current Learning Focus:** Cryptographic Primitives + Foundations
- **Target Role:** Security Analyst / Cryptography Researcher

---

## 📊 Key Takeaways

- **Never use deprecated algorithms:** DES is insecure; always prefer AES for symmetric encryption.
- **Encryption is not just hiding data:** It involves standard algorithms that, when implemented correctly, ensure confidentiality, integrity, and authenticity.
- **Mathematical Literacy:** Basic operations like XOR and Modulo are the building blocks of most modern cryptographic protocols.
- **Regulatory Compliance:** Security standards like PCI DSS dictate the necessary cryptographic protections for business data.

---

## 🧠 Final Summary

Cryptography Basics provides the essential vocabulary and mathematical foundation required to understand how digital information is protected. Moving from simple historical substitution to robust modern standards like AES illustrates the constant evolution of data security in an increasingly connected world.

---

**Task Status: ✔ Completed**
