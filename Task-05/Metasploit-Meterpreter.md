# 🐚 Metasploit: Meterpreter Runbook

## 📝 Executive Summary
Meterpreter is an advanced, dynamically extensible payload that operates entirely in memory. This runbook covers the transition from initial SMB exploitation to post-exploitation data gathering and credential extraction, highlighting the power of Metasploit's post-exploitation modules.

---

## 🌐 Core Concepts
* **In-Memory Execution:** Meterpreter resides in the target's memory, reducing the footprint on the disk and making it harder for signature-based detection to trigger.
* **Post-Exploitation Modules:** Specialized tools within Metasploit designed to gather intelligence, enumerate shares, and escalate privileges once a foothold is established.
* **Persistence & Stability:** Meterpreter sessions provide a robust environment for running commands, migrating processes, and dumping credentials without the instability of a standard shell.

---

## 🛠️ Practical Reference

### Initial Access & Enumeration
1.  **Exploit:** Use `exploit/windows/smb/psexec` (requires valid credentials).
2.  **Set Parameters:**
    *   `RHOSTS`: Target IP
    *   `SMBUser`: Username
    *   `SMBPass`: Password
3.  **Execution:** Run `exploit` to initiate the session.
4.  **System Info:** Run `sysinfo` to confirm architecture and OS version.

### Post-Exploitation Workflow
* **Backgrounding:** Use `background` to move the current session to the background, allowing you to run post-modules.
* **Enumeration:** Use `post/windows/gather/enum_shares` to identify exposed network shares.
* **Process Manipulation:** Identify `lsass.exe` to target memory-resident credentials.
* **Credential Extraction:**
    * Run `hashdump` to retrieve local account NTLM hashes.
    * Use online resources like **CrackStation** to recover cleartext passwords from cracked hashes.
* **File Searching:** Use the `search` command within Meterpreter:
    * `search -f <filename>`
    * `search -f *.txt`

### Command Bridge
* **Meterpreter to Windows CLI:** Use `shell` to drop into the native Windows Command Prompt if you need to use standard utilities like `type` (for reading file contents).

---

## 🛡️ Remediation & Security Posture
* **Credential Protection:** `lsass.exe` is a high-value target for attackers. Implement **Credential Guard** and ensure the latest security updates are applied to protect the Local Security Authority process.
* **Principle of Least Privilege:** Users should not be local administrators. Limit administrative rights to prevent `psexec` style attacks.
* **Monitoring:** Audit for unusual SMB activity or unauthorized processes accessing `lsass.exe`.
* **File Security:** Avoid storing cleartext passwords in `.txt` files on file shares or user home directories.

---

## 🧠 Key Takeaways
* **Stability:** Meterpreter is significantly more stable and feature-rich than a standard reverse shell.
* **Automation:** Metasploit's post-exploitation library allows you to automate common gathering tasks, drastically reducing time spent on manual enumeration.
* **Hash Dumping:** Obtaining the NTLM hash via `hashdump` is often the "gold key" in internal penetration testing, leading to full domain or local system compromise.

---
**Task Status:** ✅ Completed