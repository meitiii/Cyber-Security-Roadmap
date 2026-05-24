````markdown id="nmbmqq"
# 🔐 TryHackMe — Authentication Bypass Walkthrough (Summary)

This module covers the "Authentication Bypass" room on TryHackMe, which focuses on identifying weaknesses in authentication workflows and exploiting flawed verification logic to gain unauthorized access to user accounts and privileged application functionality.

Improperly secured authentication mechanisms can lead to account compromise, privilege escalation, session hijacking, and large-scale exposure of sensitive customer data.

---

## 🔍 Core Authentication Bypass Vectors

Authentication bypass vulnerabilities emerge when applications improperly validate identity, trust user-controlled input, or expose sensitive behavioral differences during authentication flows.

### Common Attack Surfaces

- **Username Enumeration:** Leaking valid usernames through verbose application responses.
- **Brute Force Attacks:** Systematically testing username/password combinations using automation tools.
- **Business Logic Flaws:** Exploiting weaknesses in transactional flows such as password reset mechanisms.
- **Cookie Tampering:** Manipulating insecure client-side session identifiers or serialized state values.

---

## 🛠️ Deep Dive: Exploitation Techniques & Tasks

### 1. Username Enumeration via Response Analysis

- **Mechanism:** Authentication and signup forms often reveal whether a username exists through inconsistent error messages.

- **Detection Example:**
    - `"User already exists"`
    - `"Invalid credentials"`

- **Security Risk:** Attackers can harvest valid usernames before launching password attacks.

---

### Enumeration Using FFUF

The room demonstrates automating username discovery using `ffuf` and a predefined username wordlist.

```bash
ffuf -w names.txt:FUZZ -u http://<TARGET_IP>/signup -X POST -d "username=FUZZ&password=test123" -H "Content-Type: application/x-www-form-urlencoded" -mr "already exists"
```

### Key Technique

- The `-mr` parameter filters responses containing a unique match string.
- Successful matches identify usernames already present in the backend database.

### Result

```text
admin
```

A valid administrative username was successfully enumerated.

---

## 🤖 Automated Brute Force Attacks

Once valid usernames are collected, attackers can automate password guessing attacks using common credential wordlists.

---

### Brute Force Execution

```bash
ffuf -w valid_users.txt:USER -w /usr/share/wordlists/SecLists/Passwords/Common-Credentials/10k-most-common.txt:PASS -u http://<TARGET_IP>/login -X POST -d "username=USER&password=PASS" -H "Content-Type: application/x-www-form-urlencoded"
```

### Attack Workflow

1. Load valid usernames into the first fuzzing parameter.
2. Load common passwords from SecLists.
3. Submit automated POST authentication requests.
4. Identify successful login responses based on status codes or response lengths.

### Outcome

- Valid credentials were identified.
- Unauthorized dashboard access was achieved.

---

## 🔄 Exploiting Password Reset Logic Flaws

### Vulnerability Overview

The password reset functionality failed to verify whether the submitted email address actually belonged to the target account.

The backend trusted user-supplied parameters without performing proper relational validation.

---

### Exploitation Workflow

#### Step 1 — Register Attacker Account

Create a controlled account:

```text
test@customer.acmeitsupport.thm
```

---

#### Step 2 — Manipulate Reset Request

Intercept or recreate the password reset request while modifying:

- **Victim Username:** Remains unchanged.
- **Destination Email:** Replaced with attacker-controlled email.

---

#### Step 3 — Token Delivery Abuse

The backend generated a valid reset token but delivered it to the attacker’s support dashboard instead of the legitimate account owner.

---

#### Step 4 — Account Takeover

- Access the generated support ticket.
- Open the password reset link.
- Assign a new password to the victim account.

### Security Impact

This vulnerability resulted in full account takeover without knowing the victim’s password.

---

## 🍪 Cookie Tampering & Session Manipulation

### Vulnerability Overview

Applications frequently store session state inside cookies. If these values are weakly encoded, unsigned, or predictable, attackers can manipulate them directly.

---

## 🔬 Session Analysis Workflow

| Auditing Step | Technical Action | Objective |
| :--- | :--- | :--- |
| Inspect Cookie | Review `Set-Cookie` headers using cURL or browser tools | Identify session structures |
| Crack Weak Hashes | Submit values to tools like CrackStation | Reverse insecure MD5 hashes |
| Decode Base64 | Decode serialized cookie values | Reveal plaintext application state |
| Active Tampering | Modify values and re-encode them | Escalate privileges or impersonate users |

---

### Practical Attack Scenario

#### Original Cookie

```text
user=guest
```

#### Tampered Cookie

```text
user=admin
```

After re-encoding and replacing the browser cookie, the application accepted the manipulated identity and granted elevated access.

---

## ⚠️ Security Impact of Authentication Bypass

Successful authentication bypass attacks may lead to:

- Full account compromise
- Administrative access
- Session hijacking
- Privilege escalation
- Unauthorized password resets
- Exposure of Personally Identifiable Information (PII)

---

## 🛡️ Remediation & Defensive Security Baselines

To defend against authentication bypass vulnerabilities, applications should implement strong server-side security controls.

---

### Generic Error Responses

Prevent username enumeration by returning identical responses for all authentication failures.

#### Recommended Example

```text
Invalid username or password
```

---

### Rate Limiting & Account Lockouts

Protect:
- `/login`
- `/signup`
- `/reset`

against automated attacks using:
- IP throttling
- CAPTCHA
- Temporary account lockouts

---

### Strict Server-Side Validation

Password reset functionality must verify:

- The email address belongs to the target account.
- Reset tokens are cryptographically bound to the correct user.

Applications must never trust user-controlled request parameters without contextual verification.

---

### Secure Session Management

Avoid weak session implementations such as:
- Plain Base64 encoding
- MD5 hashing
- Predictable serialized values

Instead use:
- Signed session tokens
- Secure server-side sessions
- Cryptographically protected JWTs

---

## 📊 Key Takeaways

- Authentication systems fail most often through logic flaws rather than cryptographic weaknesses.
- Username enumeration dramatically improves brute-force efficiency.
- Password reset mechanisms are high-value attack surfaces.
- Client-side cookies should never contain trusted authorization state.
- Weak session encoding enables privilege escalation and impersonation attacks.
- Security controls must always be enforced server-side.

---

## 🧠 Final Summary

The Authentication Bypass room demonstrates how attackers exploit flawed authentication workflows to gain unauthorized access to accounts and administrative functionality.

By abusing verbose error messages, automating brute-force attacks, manipulating password reset logic, and tampering with insecure cookies, penetration testers can bypass weak identity verification systems and compromise sensitive user accounts.

The module highlights the importance of strong server-side validation, secure session handling, generic authentication responses, and rate limiting to protect authentication infrastructure against modern attack techniques.

---

**Task Status: ✔ Completed**
````
