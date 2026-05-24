# 🛡️ Kenobi — TryHackMe (Summary)

This room provides a practical walkthrough of a Linux-based machine to practice fundamental penetration testing skills. It covers network enumeration, exploiting vulnerable services to gain initial access, and performing privilege escalation through SUID binaries and PATH manipulation.

---

## 🔍 Enumeration Basics

Proper enumeration is the foundation of this challenge, focusing on identifying open services and accessible network shares:

- **Network Scanning:** Use `nmap` to discover open TCP ports and identify services running on the machine.
- **Samba Enumeration:** Utilize `nmap` scripts (`smb-enum-shares`) to identify available shares. The `anonymous` share is a common entry point for sensitive information.
- **NFS Enumeration:** Use `rpcbind` (port 111) to identify mountable network file systems, which can provide a way to access files outside of the FTP environment.

---

## 🛠️ Escalation & Exploitation

### 1. Initial Access via ProFTPD
- **Service Versioning:** Connect via `netcat` to identify the service version (ProFTPD 1.3.5).
- **Exploitation:** Use `searchsploit` to identify vulnerabilities. The `mod_copy` module allows unauthenticated users to copy files to writable directories, such as `/var/tmp`.
- **Credential Theft:** Leverage the copy module to move an SSH private key (`id_rsa`) to a shared directory, mount that directory locally, and log in as the target user.

### 2. Privilege Escalation via SUID
- **Discovery:** Identify binaries with SUID bits set using `find / -perm -u=s -type f 2>/dev/null`.
- **Analysis:** Examine suspicious binaries (e.g., `/usr/bin/menu`) to see how they interact with the system. Using `strings` can reveal if the binary calls system commands without absolute paths.

### 3. PATH Variable Hijacking
- **Mechanism:** If a SUID binary calls a command like `curl` without its full path, the system searches the `PATH` environment variable.
- **Manipulation:** Create a malicious file named `curl` that triggers a shell (`/bin/sh`), give it executable permissions, and prepend its directory to the `PATH`.
- **Execution:** When the SUID binary runs, it executes your custom `curl` with root privileges, resulting in a root shell.

---

## 🧠 Career Direction Insight

This room reinforces critical skills for identifying misconfigured services and understanding how environment variables can be abused.

- **Current Learning Focus:** Service Enumeration + Exploitation + Privilege Escalation
- **Target Role:** Junior Penetration Tester / Security Analyst

---

## 📊 Key Takeaways

- **Start with Enumeration:** Always verify service versions and share permissions early to identify potential exploit vectors.
- **Understand Environment Variables:** The `PATH` variable is a frequent target for privilege escalation when binaries are poorly coded.
- **Leverage Tools:** Utilize `searchsploit` for known vulnerability identification and `nmap` scripts for automated enumeration.
- **Anonymity:** Always test for "anonymous" or "guest" access on network shares, as these are common misconfigurations in vulnerable systems.

---

## 🧠 Final Summary

The Kenobi room serves as an excellent guide for understanding the lifecycle of a penetration test, from initial information gathering to gaining root access. It highlights that even simple, misconfigured services and insecurely written binaries can provide full system control when exploited correctly.

---

**Task Status: ✔ Completed**