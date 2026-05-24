# 🔎 Introductory Researching — TryHackMe (Summary)

This room introduces the fundamentals of cybersecurity research skills used in penetration testing. It focuses on how to search for technical information, understand vulnerabilities, and use common security tools effectively.

---

## 🧠 Research in Cybersecurity

Pentesting relies heavily on research rather than memorization. Security analysts use documentation, vulnerability databases, and online resources to understand systems and discover security issues efficiently.

Key concepts include:
- Searching for vulnerabilities using CVE databases
- Understanding system behavior through tools
- Interpreting security-related outputs and formats

---

## 🛠️ Common Tools & Usage

Several essential tools are introduced:

- **Burp Suite Repeater** → Used to manually resend and modify HTTP requests for testing
- **Netcat (nc)** → Used for manual network communication and listening on ports
- **SCP** → Secure file transfer between systems
- **fdisk** → Disk partition management tool
- **nano** → Simple text editor for Linux

These tools are commonly used in both system administration and penetration testing workflows.

---

## 🔐 Password Hash Formats

Understanding password storage formats helps in security analysis:

- **NTLM** → Windows password hashing format
- **SHA512crypt** → Unix-based hashing format (starts with `$6$`)

---

## 🧾 Vulnerability Research (CVE System)

The CVE system tracks publicly known security vulnerabilities.

Examples:
- CVE-2020-10385 → WPForms XSS vulnerability
- CVE-2016-1240 → Apache Tomcat privilege escalation
- CVE-2007-0017 → First VLC media player CVE
- CVE-2019-18634 → sudo buffer overflow vulnerability

CVE identifiers are essential for researching and referencing security issues.

---

## 💻 Linux & Networking Commands

Important command-line utilities:

- `cron jobs` → Scheduled tasks in Linux
- `scp -r` → Copy entire directories
- `fdisk -l` → List disk partitions
- `nano -b` → Open file with backup option
- `nc -l -p 12345` → Start Netcat listener on port 12345

These are widely used in real-world security and system administration tasks.

---

## 📊 Key Takeaways

- Research skills are critical in cybersecurity
- CVEs are used to identify and track vulnerabilities
- Tools like Burp Suite, Netcat, and SCP are essential for pentesting
- Linux command-line knowledge is required for practical security work

---

## 🧠 Final Summary

This room builds the foundation of a research-oriented mindset in cybersecurity, teaching how to locate, understand, and apply technical security information effectively.

---

**Task Completed: ✔**