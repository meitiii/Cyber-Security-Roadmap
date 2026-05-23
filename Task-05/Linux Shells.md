# 🐧 Linux Shells & Scripting — Runbook

## 📝 Executive Summary
This runbook covers the fundamentals of interacting with Linux shells, navigating the filesystem, and automating tasks using Bash scripting. The shell serves as the primary interface between the user and the operating system, offering superior efficiency and resource management compared to GUI alternatives.

---

## 🌐 Core Concepts
* **Shell:** The command-line interpreter (e.g., Bash, Zsh, Fish) that executes commands provided by the user.
* **Navigation:** The ability to move through the directory structure and list file contents.
* **Searching:** Using powerful utilities like `grep` to extract patterns and data from files.
* **Scripting:** Automating repetitive tasks by combining commands into an executable file with logic (variables, loops, and conditionals).

---

## 🛠️ Practical Reference

### Essential Shell Commands
* **Identify Current Shell:** `echo $SHELL`
* **List Available Shells:** `cat /etc/shells`
* **Directory Navigation:** `pwd` (print working directory), `cd <dir>` (change directory), `ls` (list contents).
* **File Interaction:** `cat <file>` (display content), `grep "<pattern>" <file>` (search for patterns).
* **Command History:** `history` (shows previously executed commands).

### Scripting Components
* **Shebang:** `#!/bin/bash` (defines the interpreter).
* **Permissions:** `chmod +x <script.sh>` (makes the script executable).
* **Execution:** `./<script.sh>`
* **Variables:** `read <var_name>` (take user input), `$var_name` (reference variable).
* **Loops:** `for i in {start..end}; do ... done`
* **Conditionals:** `if [ "$a" = "b" ]; then ... else ... fi`

---

## 🛡️ Remediation & Best Practices
* **Privilege Management:** Use `sudo su` to escalate to root when performing system-wide searches (e.g., in `/var/log`).
* **Debugging Scripts:** Always add comments (`#`) to explain complex logic in your scripts.
* **Safety:** Use `grep -q` (quiet mode) in scripts when you only need to verify if a pattern exists without printing the output to the terminal.
* **Automation:** Store recurring tasks in scripts to reduce human error and ensure consistency in server configurations.

---

## 🧠 Key Takeaways
* **Flexibility:** Bash is the standard for system administration, while Fish offers user-friendly features like syntax highlighting, and Zsh provides extreme customization.
* **Automation:** If you find yourself typing a sequence of commands more than twice, write a script.
* **Logic:** Conditionals and loops are the backbone of smart scripts, allowing your code to adapt to different system states or user inputs.

---
**Task Status:** ✅ Completed