# 💼 OWASP Top 10 — TryHackMe (Summary)

This room provides hands-on experience with the 10 most critical web security risks as defined by the Open Worldwide Application Security Project (OWASP). It breaks down the theory behind each vulnerability and requires exploiting practical examples ranging from command injection to insecure deserialization.

---

## 💥 Injection & Authentication
Vulnerabilities occur when user-controlled input is mishandled by the application, or when identity controls are improperly implemented.

- **Command Injection:** Exploited an endpoint to execute arbitrary OS commands, allowing enumeration of the file system (discovering `drpepper.txt`) and user privileges (revealing the application runs as the low-privileged `www-data` user on Ubuntu 18.04.4).
- **Broken Authentication:** Bypassed identity controls during registration (e.g., registering with a leading space like `" darren"`) to access existing user accounts and retrieve sensitive flags.

---

## 🗄️ Data Exposure & XXE
Failing to protect sensitive data at rest or securely parse incoming XML structures frequently leads to total compromise.

- **Sensitive Data Exposure:** Discovered an exposed `/assets` directory containing a SQLite database (`webapp.db`). By extracting the database and cracking the MD5 password hash (`6eea9b...` -> `qwertyuiop`), administrative access was obtained.
- **XML External Entity (XXE):** Abused insecure XML parsing logic using custom `!ENTITY` payloads to read local files on the server, successfully extracting the user "falcon's" private SSH key (`id_rsa`).

---

## 🎭 Access Control, XSS & Deserialization
Flaws in authorization, output encoding, and object handling allow attackers to view unauthorized data, execute malicious scripts, or crash services.

- **Broken Access Control (IDOR):** Manipulated the `note` URL parameter (`?note=0`) to view notes belonging to other users, a classic Insecure Direct Object Reference.
- **Cross-Site Scripting (XSS):** Executed multiple forms of XSS (Reflected, Stored, and DOM-based) to trigger alerts, steal document cookies, and modify page content.
- **Insecure Deserialization:** Analyzed how serialized objects (often sent as base64-encoded strings in cookies) can be tampered with. By decoding and modifying these cookies, an attacker can bypass authentication or achieve Remote Code Execution.

---

## 🧩 Components with Known Vulnerabilities & Logging
Relying on outdated software or failing to monitor malicious activity provides easy attack vectors.

- **Exploiting Known Vulns:** Identified the application was running an outdated "Online Book Store 1.0" component. Used `searchsploit` to locate a known unauthenticated Remote Code Execution (RCE) exploit (EDB-ID 47887) and uploaded a PHP web shell to compromise the host.
- **Insufficient Logging:** Examined logs to identify an active brute-force attack originating from the IP `49.99.13.16`. Without adequate monitoring, these attacks go unnoticed until it is too late.

---

## 🧠 Career Direction Insight
This room is a foundational pillar for transitioning from infrastructure management to Application Security (AppSec).

- **Current Learning Focus:** Understanding how web applications mishandle input, identity, and backend logic.
- **Application:** For an engineer accustomed to securing network routes and infrastructure, AppSec highlights that firewalls cannot protect against application-layer flaws. If an application blindly executes an injected system command or deserializes malicious objects, the underlying infrastructure's security posture is rendered irrelevant. Mastering these top 10 risks is essential for secure coding, vulnerability assessment, and threat modeling.

---

## 📊 Key Takeaways
- **Never Trust User Input:** Whether it's a URL parameter, an XML file, a username, or a cookie, all input must be strictly sanitized and validated server-side.
- **Data Protection at Rest:** Storing databases in publicly accessible directories like `/assets` or using weak hashing algorithms guarantees an eventual breach.
- **Dependency Management:** Regularly auditing and updating third-party components is just as critical as writing secure custom code.

---

## 🧠 Final Summary
The OWASP Top 10 serves as the definitive guide to web application vulnerabilities. By exploiting these flaws in a controlled environment, security professionals learn exactly how misconfigurations, flawed logic, and unvalidated input can be chained together to completely compromise a target system from the outside in.

---

**Task Status: ✔ Completed**
