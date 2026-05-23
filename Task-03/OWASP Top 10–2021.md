# 💼 OWASP Top 10-2021 — TryHackMe

The 2021 revision of the OWASP Top 10 reflects a shift toward application architecture and data integrity. This room demonstrates how design flaws, improper access controls, and misconfigured infrastructure can compromise a web application, requiring hands-on exploitation for each vulnerability category.

---

## 🔑 Access, Identity, & Design Flaws
Flaws in this category stem from structural logic errors rather than traditional coding bugs.

*   **Broken Access Control (IDOR):** An Insecure Direct Object Reference allowed viewing unauthorized data by simply incrementing a numeric value in the URL (`?note=0` to `?note=1`).
*   **Insecure Design:** Bypassed a password reset mechanism by guessing a weak security question ("green"). This highlights a fundamental flaw in the application's authentication workflow.
*   **Identification & Authentication Failures:** Registration logic failed to sanitize input properly. Registering an account with a leading space (`" darren"`) tricked the database into creating a duplicate session that granted access to the original user's sensitive data.

---

## 🗄️ Cryptographic & Data Integrity Failures
When designing backend systems—like a robust SQL Server database—protecting data at rest and ensuring token integrity are just as critical as the schema itself.

*   **Cryptographic Failures:** The application carelessly stored a flat-file database (`webapp.db`) in a publicly accessible `/assets` directory. Extracting the database revealed MD5 hashes that were easily cracked to obtain the admin's plaintext password.
*   **Data & Software Integrity Failures:** The application relied on JSON Web Tokens (JWT) without enforcing signature validation. By decoding the base64 payload, changing the user to `admin`, setting the algorithm header to `none`, and dropping the signature, the token was successfully forged.

---

## 💻 Injection & Misconfigurations
Even with a solid network topology, the application layer remains vulnerable if it blindly executes input or exposes debug environments.

*   **Command Injection:** An endpoint allowed arbitrary OS commands. Basic terminal enumeration (`ls`, `whoami`, `cat /etc/alpine-release`) revealed the underlying Alpine Linux OS and the low-privileged `apache` service account.
*   **Security Misconfiguration:** The application left the Werkzeug debug console exposed. Because you are already comfortable writing Python, leveraging this console was trivial—importing the `os` module and using `os.popen` executed system-level commands to read the backend `app.py` source code.
*   **Server-Side Request Forgery (SSRF):** A `server` parameter intended to fetch files from `secure-file-storage.com` was manipulated. By setting up a local Netcat listener (`nc -lvp 8080`) and redirecting the parameter to the attack machine's IP, the application inadvertently handed over its hidden API keys.

---

## 🏗️ Components & Logging
*   **Vulnerable Components:** The site ran an outdated Bookstore application. A quick `searchsploit` query revealed a known Unauthenticated Remote Code Execution (RCE) script, immediately granting a shell.
*   **Logging & Monitoring Failures:** Investigating the server logs revealed a consistent pattern of failed logins every 5 minutes from a single IP address (49.99.13.16), exposing an active, unmitigated brute-force attack.

---

## 🧠 Engineering Insight
This room illustrates a vital concept: infrastructure security and application security must work in tandem. You can design a perfectly segmented OSPF/RIP network environment, but if the web application allows SSRF or exposes an unauthenticated Python debug console, an attacker can bypass those network controls entirely. The shift in the 2021 OWASP list heavily emphasizes *building* security into the architecture (Insecure Design) rather than just patching bugs after deployment.

---

**Task Status: ✔ Completed**