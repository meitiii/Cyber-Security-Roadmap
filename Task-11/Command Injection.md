# 💻 TryHackMe — OS Command Injection Walkthrough

This module covers the critical mechanics of OS Command Injection (also known as Remote Code Execution - RCE). This high-severity flaw occurs when a web application passes unsafe user-supplied data directly to a system shell, allowing attackers to execute arbitrary operating system commands under the privileges of the running application process.

---

## 🔍 Core Vulnerability Mechanisms

Command Injection represents a breakdown in input validation. When an application interacts with the underlying hosting operating system using language-specific system call wrappers (such as PHP’s `system()`, Python’s `subprocess`, or Node.js’s `child_process`), unchecked input can manipulate the command logic.

### 🧵 Shell Operators & Execution Chaining
Attackers use shell control operators to append malicious payload strings onto legitimate execution parameters:

*   `&` **(Background Operator):** Runs the appended command concurrently in the background.
*   `;` **(Sequential Delimiter):** Runs the second command regardless of whether the first command succeeds or fails.
*   `&&` **(Conditional AND):** Runs the second command only if the first command completes successfully (Exit Code = 0).

---

## 🕵️ Detection Methodologies

Command Injection generally manifests in one of two ways based on application feedback:

### 1. Verbose (In-Band) Injection
The application directly prints the `stdout`/`stderr` output of the executed command back onto the web page interface.

*   **Validation:** Sending a baseline structural query like `whoami` or `id` displays the active service account name (e.g., `www-data` or `nt authority\system`) directly inside the HTML response.

### 2. Blind Command Injection
The application processes the injected payload, but no command response text is mirrored back to the front-end browser view.

#### Validation Framework:
*   **Time-Based Probing:** Forcing execution delays using tools like `ping` (Linux) or `timeout` (Windows) to measure time deltas.
*   **Output Redirection (`>`):** Piping the command output into a static file within the public web root (e.g., `& whoami > /var/www/html/out.txt`) and checking it via a web browser.
*   **Out-of-Band (OOB) Exfiltration:** Forcing the server to trigger a network callback to an attacker-controlled listener using utilities like `curl` or `nslookup`.

---

## 🛠️ Code Snippets & Analysis

### 📁 Task 2: Code Review (PHP & Python Flask)

#### PHP Back-End Context
The application stores local MP3 assets and accepts a user-supplied search term to query a track index text file using the native OS `grep` tool.

*   **[Question 2.1]** What variable stores the user’s input in the PHP code snippet in this task?
    *   **Answer:** `title`
*   **[Question 2.2]** What HTTP method is used to retrieve data submitted by a user in the PHP code snippet?
    *   **Answer:** `GET`

#### Python Flask Context
A development route uses Python's `subprocess` engine to directly parse paths provided via the web browser address bar.

*   **[Question 2.3]** If I wanted to execute the id command in the Python code snippet, what route would I need to visit?
    *   **Answer:** `/id`

---

## 🛡️ Remediation & Defensive Security Baselines

To eliminate command injection bugs at the architectural design phase:

1.  **Avoid Native Shell Invocation:** Utilize native programming language APIs instead of system shell calls. For example, use PHP's `rename()` function instead of invoking `system("mv file1 file2")`.
2.  **Implement Strict Input Sanitization & Validation:** Enforce character whitelists that restrict input parameters strictly to safe alphanumeric ranges (e.g., digits 0-9 or regex frameworks like `^[a-zA-Z0-9]+$`), blocking terminal characters entirely.
3.  **Use Parameterized Execution APIs:** When system calls are completely unavoidable, pass variables as an array of arguments rather than a single concatenated string. This ensures the operating system handles the input strictly as data arguments, not executable commands.

---

## 🎯 Lab Q&A Verification

### Task 3: Basic Payloads
*   **[Question 3.1]** What payload would I use if I wanted to determine what user the application is running as?
    *   **Answer:** `whoami`
*   **[Question 3.2]** What popular network tool would I use to test for blind command injection on a Linux machine?
    *   **Answer:** `ping`
*   **[Question 3.3]** What payload would I use to test a Windows machine for blind command injection?
    *   **Answer:** `timeout`

### Task 4: Prevention Mechanics
*   **[Question 4.1]** What is the term for the process of “cleaning” user input that is provided to an application?
    *   **Answer:** `sanitisation`

### Task 5: Final Practical Lab Exploitation
*   **Enumeration:** Submitting an execution parameter with `& dir` successfully yields a local directory listing, confirming the underlying target host is running a Windows platform rather than Linux.
*   **[Question 5.1]** What user is this application running as?
    *   **Answer:** `www-data`
*   **[Question 5.2]** What are the contents of the flag located in `/home/tryhackme/flag.txt`?
    *   *Exploit Vector:* `& cat /home/tryhackme/flag.txt`
    *   **Answer:** `THM{COMMAND_INJECTION_COMPLETE}`

---
*   **Cheat Sheet Resource:** [PayloadsAllTheThings — Command Injection List](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection)
*   **Task Status:** ✔ Room Completed Successfully