🛡️ LazyAdmin — TryHackMe (Summary)
This room explores a full kill-chain exploitation on a vulnerable Linux machine named LazyAdmin. It covers initial reconnaissance, attacking a misconfigured SweetRice CMS via an extension-bypass file upload, credential harvesting from public database backups, and elevating privileges to root through a writable shell script invoked by an over-privileged Perl utility.

🛠️ Exploitation & Escalation Techniques
1. Reconnaissance & Enumeration
Identifying the target's attack surface and locating critical application pathways.

Port Scanning: Run nmap -sV -sC <target-ip> to discover active listeners. Ports 22 (SSH) and 80 (HTTP) are open.
Directory Discovery: Web enumeration on port 80 reveals a default page. Use Gobuster to uncover hidden file nodes:
gobuster dir -u http://<target-ip>/ -w /usr/share/wordlists/dirb/common.txt reveals a /content directory.
Recursive hunting via gobuster dir -u http://<target-ip>/content -w common.txt exposes the administrative panel entry point /content/as and an unauthenticated database archive repository inside /content/inc.
2. Information Disclosure & CMS Authentication
Harvesting administrative application credentials from unencrypted system configuration dumps.

Backup Leakage: Navigate to /content/inc and download the exposed MySQL database backup file (.sql).
Credential Cracking: Extract the admin username (manager) and an MD5 password hash directly out of the SQL text. Pass the string into a dictionary-matching asset like CrackStation to decode the plaintext sequence: Password123.
Panel Access: Authenticate successfully into the backend SweetRice CMS suite dashboard at /content/as using the recovered profile assets.
3. Arbitrary File Upload to Initial Shell (RCE)
Bypassing local application input restrictions to place an interactive reverse-shell execution thread.

Extension Evasion: The upload panel screens standard .php uploads. To bypass this mechanism, fetch the PentestMonkey PHP reverse shell script, input your local listener IP and port coordinates, and modify the filename ending sequence to an active alternative execution extension: shell.php5.
Shell Interception: Upload the payload inside the CMS dashboard attachment center. Start an active staging listener on your attack system via terminal:

Bash

nc -lvnp 4444
- **Execution Hook:** Trigger the server-side processor by accessing the uploaded file pathway at `http://<target-ip>/content/attachment/shell.php5`. This returns an interactive web-server command terminal contextualized under the low-privilege `www-data` service persona.

### 4. Privilege Escalation via Writable Script Hijacking
Exploiting weak write access rights assigned to system management automation routines.
- **Sudo Auditing:** Run `sudo -l` to see what tools the current account can launch with elevated rights. The configuration allows the execution of a custom localized script system helper without password entry requirement bounds: `sudo /usr/bin/perl /home/itguy/backup.pl`.
- **Chain Analysis:** Reading the source contents of the execution path `/home/itguy/backup.pl` shows it triggers an underlying shell script sequence located at `/etc/copy.sh`.
- **Script Poisoning:** Check filesystem permissions. The secondary referenced script `/etc/copy.sh` is world-writable. Overwrite its behavior to launch an interactive bash console loop when spawned:
```bash
  echo "/bin/bash -i" > /etc/copy.sh
Root Elevation: Run the privileged master Perl tool:

Bash

sudo /usr/bin/perl /home/itguy/backup.pl
  The script executes with automated system supervisor permissions, executing your newly injected shell line code and returning an unrestricted `root` terminal.
```
---

## 🧠 Career Direction Insight

This module provides practical hands-on experience in chaining multiple distinct low-severity flaws—directory listing, loose backup permissions, weak application extension screening, and writable scripts—into a critical multi-stage system takeover.

- **Current Learning Focus:** Full-Chain CTF Methodology & Web Application Vectors
- **Target Role:** Penetration Tester / Red Teamer

---

## 📊 Key Takeaways

- **Information disclosure leads to breach:** A simple unencrypted file asset like a backup inside public web roots can instantly surrender complete software authentication boundaries.
- **Screen extensions thoroughly:** Relying purely on basic string blocklists like `.php` without screening variations like `.php5` or `.phtml` creates immediate bypass vectors.
- **Sudo chains must be locked down:** If a program executed with `sudo` permissions interacts with or calls downstream assets, every component in that pipeline must be fully secure and non-writable by lower tiers.

---

## 🧠 Final Summary

The compromise of the LazyAdmin system highlights the catastrophic impact of cascading configuration flaws. By linking an exposed database backup with an incomplete file upload blocklist, initial user access was established. From there, weak permission management on automation scripts enabled an immediate lateral jump straight to root level system dominance.

---

**Task Status: ✔ Completed**