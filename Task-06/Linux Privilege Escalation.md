# 🛡️ Linux Privilege Escalation — TryHackMe (Summary)

This room explores advanced techniques for privilege escalation on Linux systems. It covers a wide range of vectors, from kernel-level exploits and SUID binaries to misconfigured environment variables, cron jobs, and NFS shares.

---

## 🛠️ Escalation Techniques

### 1. Kernel Exploits
Kernel vulnerabilities can lead to full system compromise. 
- **Identification:** Use `uname -a` and `cat /proc/version` to identify the kernel version.
- **Exploitation:** Search for relevant CVEs, transfer the exploit code to the target (often via a temporary Python web server), compile using `gcc`, and execute.

### 2. Sudo Misconfigurations
If a user can run specific binaries with `sudo` without a password, they can often spawn a root shell.
- **Discovery:** Run `sudo -l` to list allowed commands.
- **Exploitation:** Consult [GTFOBins](https://gtfobins.github.io/) for specific commands to escape to a shell using binaries like `less`, `nmap`, or `find`.

### 3. SUID/GUID Binaries
Files with the SUID bit set run with the permissions of the file owner (often root).
- **Discovery:** `find / -type f -perm -04000 -ls 2>/dev/null`
- **Exploitation:** Use these binaries to read sensitive files (e.g., `/etc/shadow`) or spawn shells. If `base64` has SUID, use it to read and decode shadow files to crack hashes.

### 4. Capabilities
Binary capabilities allow fine-grained access control instead of full root privileges.
- **Discovery:** `getcap -r /`
- **Exploitation:** Binaries like `vim` or `rview` with specific capabilities (e.g., `cap_setuid`) can be abused to set the UID to 0 (root) and spawn a shell.

### 5. Cron Job Abuse
Scheduled tasks running as root are high-value targets.
- **Exploitation:** If you have write access to a script run by `cron`, append a reverse shell or set SUID permissions on your own binary to gain root access when the cron job triggers.

### 6. PATH Hijacking
If a script calls a binary using a relative path (e.g., `ls` instead of `/bin/ls`), you can manipulate the `$PATH` variable.
- **Exploitation:** Create a malicious file named `ls` (containing `/bin/bash`), make it executable, and update the `$PATH` to point to your directory first.

### 7. NFS (Network File System) Misconfiguration
If an exported share has the `no_root_squash` option, the client can act as root on the target system.
- **Exploitation:** Mount the share locally, create a compiled C program with SUID permissions, set the SUID bit on your attacking machine, and then execute it on the target machine as root.

---

## 🧠 Career Direction Insight

This module is essential for mastering post-exploitation and internal penetration testing.

- **Current Learning Focus:** Post-Exploitation + Advanced Linux Security
- **Target Role:** Penetration Tester / Red Teamer

---

## 📊 Key Takeaways

- **Enumeration is everything:** Always document the kernel version, sudo rights, SUID files, and cron jobs.
- **Persistence:** Privilege escalation is the key to maintaining persistence and accessing protected data.
- **GTFOBins is your best friend:** When you find a binary you can run with elevated privileges, GTFOBins is the standard resource for finding the escape sequence.
- **Caution:** Always be careful when running kernel exploits, as they can crash target services.

---

## 🧠 Final Summary

Privilege escalation is the bridge between gaining initial access and achieving full system control. By identifying flaws in system design—such as misconfigured permissions, scheduled tasks, and vulnerable kernel versions—a tester can systematically elevate their privileges to perform administrative tasks and secure persistence.

---

**Task Status: ✔ Completed**