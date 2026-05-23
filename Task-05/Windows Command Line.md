# 💻 Windows Command Line Basics — Runbook

## 📝 Executive Summary
This runbook covers fundamental `cmd.exe` operations essential for system administration, basic forensic data gathering, and network troubleshooting in a Windows environment. Mastering these commands provides a significant speed advantage over GUI-based tools.

---

## 🌐 Core Concepts
* **CLI Efficiency:** Command-line interfaces consume fewer system resources and are more conducive to automation and remote management (e.g., via RDP or WinRM).
* **Help Resources:** Use `command /?` or `help command` to access documentation for any utility.
* **Piping:** Use the pipe operator `|` to pass the output of one command to another (e.g., `driverquery | more` to paginate long lists).

---

## 🛠️ Practical Reference

### System Information
* **OS Version:** `ver`
* **System Specs:** `systeminfo`
* **Drivers:** `driverquery | more`
* **Hostname:** `hostname` (or check via `ipconfig /all`)

### Network Troubleshooting
* **Configuration:** `ipconfig /all` (includes MAC address, DHCP, and DNS status)
* **Connectivity:** `ping <target>`
* **Route Tracing:** `tracert <target>`
* **DNS Lookup:** `nslookup <domain>`
* **Active Connections:** `netstat -abon` (shows ports and associated Process IDs)

### File & Directory Management
* **Navigation:** `cd <dir>`, `dir` (list contents), `tree` (visual directory structure)
* **Creation/Removal:** `mkdir <name>`, `rmdir <name>`, `del <file>`
* **File Operations:** `type <file>` (display contents), `copy <src> <dest>`, `move <src> <dest>`
* **Hidden Files:** `dir /a`

### Process Management
* **List Processes:** `tasklist`
* **Filter Processes:** `tasklist /FI "imagename eq <name>"`
* **Kill Process:** `taskkill /PID <PID>`

---

## 🛡️ Remediation & Best Practices
* **Standardize Troubleshooting:** Always check `ipconfig /all` first when dealing with connectivity issues to rule out IP/Gateway misconfigurations.
* **Process Auditing:** Use `netstat -abon` to identify malicious services listening on unauthorized ports. 
* **Maintenance:** Use `sfc /scannow` to detect and repair corrupted system files on unstable Windows hosts.
* **Safety:** When scheduling shutdowns or restarts, always keep the abort command (`shutdown /a`) ready to prevent accidental data loss.

---

## 🧠 Key Takeaways
* **Paging:** Always pipe long output (e.g., `| more`) to avoid losing data in the terminal buffer.
* **Automation:** These commands form the building blocks for batch scripts (`.bat` or `.cmd`) used to automate repetitive system tasks.
* **Security Context:** Note that managing drivers or system services often requires an administrative command prompt.

---
**Task Status:** ✅ Completed