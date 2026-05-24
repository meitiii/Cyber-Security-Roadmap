Plaintext

# 🔓 TryHackMe — Insecure Direct Object Reference (IDOR) Walkthrough

This module covers the "IDOR" room on TryHackMe, providing a comprehensive analysis of Insecure Direct Object Reference vulnerabilities. IDOR is a critical access control flaw occurring when an application provides direct access to objects based on user-supplied input without performing adequate server-side authorization checks.

---

## 🔍 Core Mechanisms of IDOR Vulnerabilities

An IDOR vulnerability manifests when a web server accepts direct indicators (database primary keys, filenames, account numbers) from a user to fetch data but fails to validate ownership. If a logged-in user can alter an input parameter (like an ID) and view or modify another user's restricted resource, access control is broken.

### Structural Variations of Object Identifiers

Web applications attempt to obscure internal identifiers using several patterns, requiring different auditing tactics:

1. **Clear Text (Integer IDs):** Directly predictable sequential numbers (e.g., `user_id=1001` $\rightarrow$ `user_id=1000`).
2. **Encoded IDs:** Parameters obfuscated via standardized string transformations. The most prevalent technique is **Base64 Encoding** (spotted by alphanumeric ranges and `=` padding). Attackers decode the string, manipulate the sequential values, and re-encode it to execute the bypass.
3. **Hashed IDs:** Cryptographic representations (e.g., `123` becoming the MD5 signature `202cb962ac59075b964b07152d234b70`). Testers check for predictable hashing functions by submitting values to reference platforms like **CrackStation**.
4. **Unpredictable IDs (UUIDs/GUIDs):** Non-sequential, unique values. To audit these, a minimum of **2 distinct accounts** must be created. Testers swap the unpredictable IDs between the active sessions to see if cross-account resource viewing is allowed without authorization.

---

## 🕵️ Location & Parameter Mining (Where to Look)

IDOR bugs are frequently missed because they do not always appear directly in the primary browser address bar. High-impact areas include:
* **Asynchronous Background Requests:** API endpoints running silently behind the scenes, loaded via **AJAX** requests or tracked via the browser's Network Developer Tools.
* **Hidden/Forgotten Parameters:** Legacy or development parameters accidentally left in production code. A secure endpoint like `/user/details` might become vulnerable when a hidden tracking parameter is appended (e.g., `/user/details?user_id=123`).

---

## 🛠️ Step-by-Step Task Exploitation

### Task 2: Static Example Simulation
- **Context:** Inspecting sequential order confirmations. 
- **Exploitation:** Modifying accessible URL parameters to traverse transactional states.
- **Flag Captured:** `THM{IDOR-VULN-FOUND}`

### Task 7: Practical API IDOR Lab
- **Scenario:** The target application features an account profile page where client data is dynamically pre-filled.
- **Enumeration:** 1. Open Browser Developer Tools and navigate to the **Network** tab.
  2. Refresh the account page and isolate the asynchronous backend API call.
  3. Locate the vulnerable endpoint signature: `/api/v1/customer?id={user_id}`
- **Exploitation & Data Harvesting:**
  By executing targeted modifications to the `id` query string parameter, arbitrary user data was dumped directly from the API endpoint:
  * **Targeting ID 1:** Changing parameter to `id=1` leaked account metadata for user: **`adam84`**
  * **Targeting ID 3:** Changing parameter to `id=3` leaked account metadata for email: **`j@fakemail.thm`**

---

## 🛡️ Remediation & Defensive Security Baselines

To effectively prevent and neutralize IDOR vulnerabilities at the application architecture level:

- **Enforce Robust Server-Side Authorization:** Never trust client-controlled parameters. For every request involving a resource ID, the application backend must perform a contextual verification check:
```text
  Is [Current Session User] explicitly authorized to [Read/Write/Delete] [Requested Resource ID]?
Implement Indirect Reference Maps: Avoid exposing raw database primary keys or internal sequence structures. Instead, use transient, session-scoped map keys that map back to the real resource internally on the server side.
Utilize Cryptographically Secure UUIDs: Use random, non-sequential UUIDs (v4) to prevent attackers from guessing alternate resource identifiers via brute-force or parameter fuzzing. (Note: This adds complexity to enumeration but must still be paired with proper authorization checks).
Task Status: ✔ Completed