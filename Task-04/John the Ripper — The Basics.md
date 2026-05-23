# 🗝️ John the Ripper (JtR): The Basics — Runbook

This room covers advanced password cracking techniques using JtR, including working with Linux/Windows hashes, archive files, and SSH keys.

---

## 🛠️ JtR Workflow & Utilities
* **Hash Identification:** Use `hash-id` or sites like Hashes.com.
* **Linux Hash Cracking (`/etc/shadow`):** Requires merging password and shadow files first.
  * `unshadow passwd shadow > unshadowed.txt`
  * `john --wordlist=rockyou.txt unshadowed.txt`
* **Archive & Key Extraction:** Use conversion tools to extract hashes from protected files.
  * **ZIP:** `zip2john secure.zip > zip.hash`
  * **RAR:** `rar2john secure.rar > rar.hash`
  * **SSH:** `ssh2john id_rsa > id_rsa.hash`

---

## ⌨️ Command & Tool Reference
* **Standard Crack:** `john --format=[format] --wordlist=[wordlist] [hash_file]`
* **Single Crack Mode:** Useful when you have the username.
  * Append username to file: `user:hash`
  * Run: `john --single --format=[format] [hash_file]`
* **Custom Rules:** Defined in `john.conf` to exploit password predictability.
  * **Flag:** `--rule=THMRules`
  * **Rule Example:** `Az"[A-Z]"` (Adds all capital letters to the end of a word).

---

## ⚙️ Key Formats
| Hash/System | JtR Format Flag |
| :--- | :--- |
| **Windows NTLM** | `nt` |
| **MD5** | `raw-md5` |
| **SHA-1** | `raw-sha1` |
| **SHA-256** | `raw-sha256` |
| **Whirlpool** | `whirlpool` |

---

## 🛡️ Remediation (Defense)
* **Strong Password Policies:** Implement length and complexity requirements to negate the efficiency of wordlist-based attacks (RockYou).
* **Salted Hashes:** Ensure hashes are salted at the system level to prevent easy lookup of common passwords.
* **Principle of Least Privilege:** Restrict access to `/etc/shadow` and SAM files to prevent attackers from obtaining hashes in the first place.
* **Archive Security:** Use strong encryption (AES-256) for archives and avoid storing passwords in plain text on the file system.

---

## 🧠 Key Takeaways
* **Pre-Processing is Mandatory:** Tools like `unshadow`, `zip2john`, and `rar2john` are essential because John cannot crack raw ZIP/RAR files or `/etc/shadow` entries without first extracting the hash into a compatible format.
* **Single Crack Mode:** When you have a username (e.g., "Joker"), JtR's `--single` mode is significantly faster than wordlist attacks because it intelligently mangles the username to guess the password.
* **Custom Rules:** When wordlists fail, custom rules allow you to simulate human password habits (e.g., adding numbers or symbols to the end of words), which often leads to success.

---
**Task Status:** ✅ Completed