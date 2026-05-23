# 🎓 Vulnversity — TryHackMe (Summary)

This room is a classic CTF exercise that guides learners through a standard web application exploitation workflow: reconnaissance, bypass of file upload restrictions, reverse shell deployment, and privilege escalation using SUID binaries.

---

## 🔍 Enumeration & Reconnaissance

- **NMAP Scan:** Identify open services. Look for web servers (e.g., port 3333).
- **Directory Brute-forcing:** Use tools like `Gobuster` or `FFUF` with `SecLists` (directory-list-2.3-medium.txt) to discover hidden paths.
- **Hidden Directories:** The `/internal` directory often serves as the entry point for file upload vulnerabilities.

---

## 🛠️ Exploitation: Bypassing Restrictions

The target may block standard `.php` files during upload. 

1. **Fuzzing Extensions:** Use `Burp Suite Intruder` to test which file extensions the server accepts. Replace the extension of a reverse shell payload with items from `web-extensions.txt`.
2. **Detection:** Look for an extension that yields a different HTTP response length (e.g., `.phtml`).
3. **Execution:** 
   - Rename the payload (e.g., `shell.phtml`).
   - Update the IP and port in the script to match your attacking machine (`tun0` IP).
   - Start a listener: `nc -nvlp 1234`.
   - Upload the file and navigate to `/internal/uploads/[filename]` to trigger the reverse shell.

---

## 🚀 Privilege Escalation

Once initial access is achieved as a low-privileged user, look for SUID binaries that can be abused.

- **Search:** `find / -type f -perm -04000 -ls 2>/dev/null`
- **Target:** `systemctl` is a powerful binary if misconfigured with SUID.
- **Exploitation:** Use [GTFOBins](https://gtfobins.github.io/) to find methods to execute commands as root.
- **Systemd Service Trick:**
  1. Create a malicious service file:
```bash
     TF=$(mktemp).service
     echo '[Service]
     Type=oneshot
     ExecStart=/bin/sh -c "cat /root/root.txt > /tmp/output"
     [Install]
     WantedBy=multi-user.target' > $TF
     ```
  2. Use `systemctl` to link and enable the service, which executes the command as root.
  3. Retrieve the flag: `cat /tmp/output`.

---

## 🧠 Career Direction Insight

This room reinforces the methodology used in real-world web application penetration testing.

- **Current Learning Focus:** Web Exploitation + Post-Exploitation
- **Target Role:** Penetration Tester / Security Consultant

---

## 📊 Key Takeaways

- **Don't assume extensions:** If `.php` is blocked, fuzz for alternatives like `.phtml`, `.php5`, or `.phar`.
- **Burp Suite is essential:** Mastering `Intruder` for fuzzing is a critical skill for bypassing upload filters.
- **GTFOBins is the standard:** Always check discovered SUID binaries against GTFOBins to identify potential escalation paths.
- **Systemd abuse:** Abusing `systemctl` is a sophisticated way to gain root persistence and execution when permissions are loose.

---

## 🧠 Final Summary

Vulnversity provides an excellent blueprint for a typical penetration test: finding an entry point via a web application, handling restrictive input sanitization, and escalating privileges through misconfigured system binaries. Understanding this flow is fundamental for transitioning from basic hacking to more advanced red teaming activities.

---

**Task Status: ✔ Completed**