# 🌐 TryHackMe — Server-Side Request Forgery (SSRF) Walkthrough

This module covers the fundamental concepts of Server-Side Request Forgery (SSRF), detailing how attackers exploit trusted server-to-server communications to map out internal networks, extract metadata, and bypass perimeter defenses.

---

## 🔍 Task 1: What is an SSRF?

### 📁 Core Vulnerability Mechanism
Server-Side Request Forgery (SSRF) is a web security vulnerability that allows an attacker to induce the server-side application to make HTTP requests to an arbitrary domain of the attacker's choosing.

Because the request originates from the trusted back-end server, it can bypass traditional network controls (like firewalls or ACLs) to interact with internal-only infrastructure.

### ⚔️ Classification & Impact Analysis

#### 1. Architectural Variations
*   **Regular (In-Band) SSRF:** The attacker receives a direct response containing data or HTML elements leaked from the target internal system back into the front-end interface.
*   **Blind SSRF:** The back-end server processes the request and executes the network connection, but no transaction data or response payload is mirrored back to the attacker's view. *(Often validated via out-of-band monitoring tools like Burp Collaborator)*.

#### 2. High-Risk Impact Vectors
A successful SSRF exploit sequence can easily escalate to a critical business compromise by achieving:
*   **Internal Network Mapping:** Forcing the server to scan private subnets to discover active internal services and open ports.
*   **Data Leakage:** Accessing locally stored sensitive configuration files, API tokens, or hidden administration panels.
*   **Cloud Infrastructure Takeover:** Accessing local cloud metadata endpoints to extract long-term orchestration keys and instance credentials.

---

## 🛠️ Practical Exploitation Vectors

If an application features a functionality that fetches remote URLs (e.g., pulling a profile picture from an external link), attackers can manipulate the parameter values:

### Case A: Internal Loopback Enumeration
```http
GET /fetch?url=http://localhost/admin HTTP/1.1
Host: vulnerable-target.thm
```
*   **🎯 Result:** Forces the application server to query its own local interface, potentially exposing access-controlled administrative workflows without authentication.

### Case B: Cloud Metadata Service Harvesting
```http
GET /fetch?url=[http://169.254.169.254/latest/meta-data/](http://169.254.169.254/latest/meta-data/) HTTP/1.1
Host: vulnerable-target.thm
```
*   **🎯 Result:** Queries the link-local cloud infrastructure address, exposing temporary IAM credentials and API tokens in AWS, GCP, or Azure environments.

---

## 🛡️ Remediation & Defensive Security Baselines

To robustly secure applications against inbound SSRF manipulation at the code level:

1.  **Enforce Strict Whitelisting:** Implement a strict, predefined list of approved domains or IP addresses that the application is allowed to query. Reject all other inputs by default.
2.  **Block Internal Routing Ranges:** Configure the input verification mechanism to drop requests targeting loopback addresses (`127.0.0.1`) or private subnets defined in RFC 1918 (`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`).
3.  **Disable Dangerous Protocols:** Strict filtering should only permit standard `http://` or `https://` schemas. Completely disable or strip alternative protocol streams such as `file://`, `gopher://`, or `dict://`.
4.  **Network Segmentation:** Restrict outbound network access directly from the application layer so it cannot physically communicate with non-essential internal microservices.

---

## 🎯 Lab Q&A Verification

*   **[Question 1.1]** What does SSRF stand for?
    *   **Answer:** `Server-Side Request Forgery`
*   **[Question 1.2]** As opposed to a regular SSRF, what is the other type?
    *   **Answer:** `Blind`

---
*   **Lab Access Link:** [TryHackMe SSRF Room](https://tryhackme.com/room/ssrf)
*   **Task Status:** ✔ Section 1 Completed