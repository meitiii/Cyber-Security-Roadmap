# 🗺️ TryHackMe — Walking An Application Walkthrough (Summary)

This module covers the "Walking An Application" room on TryHackMe, which focuses on manually reviewing web applications for security issues using only built-in browser developer tools. Relying solely on automated tools often leads to missing crucial logical flaws, hidden paths, and information leakage.

---

## 🔍 Understanding Manual Application Auditing

Walking an application is the process of manually reviewing a web application's visible footprint to discover features, map components, and identify potential vulnerabilities.

- **Core Mechanism:** Using native browser features to audit client-side code, manipulate styles, inspect live components, freeze execution threads, and analyze transactional traffic.
- **Potential Impact of Discovered Issues:**
    - **Information Disclosure:** Uncovering hardcoded paths, developer logic, credentials, or internal updates in source comments.
    - **Insecure Direct Object References (IDOR) & Hidden Paths:** Discovering private pages or exposed system archives.
    - **Improper Directory Permissions:** Gaining directory listing access to sensitive system asset folders.
    - **Client-Side Authorization Bypasses:** Circumventing UI blocks (paywalls) or input rules directly via DOM manipulation.

---

## 🛠️ Browser-Based Auditing Techniques

### 1. Viewing Static Page Source
Analyzing the raw human-readable code (HTML, CSS, JavaScript) returned by the server via `Ctrl + U` or the `view-source:` URI scheme.

### 2. Live DOM Manipulation (Inspector)
Inspecting page elements dynamically. Since modern web frameworks change content in real-time, the Inspector provides a live view of the DOM and allows testers to alter element attributes or CSS classes to bypass client-side limitations.

### 3. Client-Side Flow Control (Debugger)
Analyzing and controlling the execution of client-side scripts. Testers can use tools like "Pretty Print" to de-obfuscate minified JavaScript files and set execution breakpoints.

### 4. Traffic Interception (Network Monitor)
Monitoring all network requests made by the page, specifically tracking background requests like AJAX/XHR connections that happen asynchronously without causing a full page refresh.

---

## 📋 Comprehensive Task Walkthrough & Flag Discovery

### Task 1 & Task 2: Environment Provisioning & Feature Mapping
- **Action:** Start the target machine and navigate to the assigned URL (`https://<LAB_IP>.p.thmlabs.com`).
- **Objective:** Act as a penetration tester by manually visiting available pages, listing active endpoints, and noting user interaction vectors (e.g., support forms, news feeds).
- **Status:** Completed.

### Task 3: Viewing The Page Source (Static Analysis)
By opening the page source on the application homepage, multiple misconfigurations and information leaks were found:

| Target Vector / Clue Found | Exploitation Method | Flag Recovered |
| :--- | :--- | :--- |
| **Developer Comment:** `<!-- This page is temporary while we work on the new homepage @ /new-home-beta -->` | Manually appended the hidden development directory to the base URL: `/new-home-beta` | `THM{HTML_COMMENTS_ARE_DANGEROUS}` |
| **Hidden Anchor Tag:** An HTML link starting with `secr` | Located the specific hidden path link inside the source code and visited it directly. | `THM{NOT_A_SECRET_ANYMORE}` |
| **Asset Directory Exposure:** Static file folders handling CSS/JS includes. | Navigated directly to the parent directory `/assets/`. Due to directory listing being enabled, accessed the raw file structure and opened `flag.txt`. | `THM{INVALID_DIRECTORY_PERMISSIONS}` |
| **Framework Comment:** Notice referencing the framework version and external updates website. | Followed the link to the framework developer site, reviewed the public **Change Log**, noticed an application backup reference to `/tmp.zip`, downloaded it, and unzipped the text asset. | `THM{KEEP_YOUR_SOFTWARE_UPDATED}` |

### Task 4: Developer Tools — Inspector (Bypassing Paywalls)
- **Detection:** Navigating to the news section reveals a third premium article blocked by a dynamic UI overlay component (Paywall).
- **Exploitation:** 
    1. Right-clicked the floating blocker element and selected **Inspect**.
    2. Located the target container element: `<div class="premium-customer-blocker">`.
    3. Checked the CSS style declarations block and targeted the attribute `display: block;`.
    4. Modified the value to `none` (or removed the attribute completely). The overlay vanished, revealing the text underneath.
- **Flag Recovered:** `THM{NOT_SO_HIDDEN}`

### Task 5: Developer Tools — Debugger (JavaScript Breakpoints)
- **Detection:** Loading the contact page causes a rapid flash of a red popup window that immediately disappears from view via automated client-side JavaScript functions.
- **Exploitation:**
    1. Opened the **Debugger** (or **Sources** tab in Chrome) and selected `assets/flash.min.js`.
    2. Applied **Pretty Print** (`{ }` icon) to expand and format the minified/obfuscated JavaScript text.
    3. Located the cleanup logic handling element removal at the bottom of the script.
    4. Placed a runtime **Breakpoint** on the specific execution line (turning the line indicator blue).
    5. Refreshed the page; the execution thread paused instantly, freezing the red box on screen before deletion.
- **Flag Recovered:** `THM{CATCH_ME_IF_YOU_CAN}`

### Task 6: Developer Tools — Network (Asynchronous AJAX Ingestion)
- **Detection:** Forms that interact with a database asynchronously use background APIs without triggering a browser-level page reload.
- **Exploitation:**
    1. Opened the **Network** tab in developer tools.
    2. Populated the fields of the contact support form (Name, Email, Message) with test inputs.
    3. Clicked **Send Message** and monitored the live network request stream.
    4. Inspected the newly generated asynchronous backend connection file named `contact.msg`.
    5. Read the raw text inside the **Response** tab of the payload stream.
- **Flag Recovered:** `THM{GOT_AJAX_FLAG}`

---

## 🛡️ Prevention & Remediation Strategies

To mitigate vulnerabilities exposed during client-side auditing, applications should implement proper server-side control baselines:

- **Disable Directory Listing:** Configure web servers (Apache, Nginx, IIS) to reject direct path indexing on asset directories (returning a `403 Forbidden` error instead of rendering folder directories).
- **Secure Information Disclosure:** Strip developer comments, debug notes, testing paths, and framework versions from production-deployed client-side code during compilation pipelines.
- **Server-Side Validation:** Never rely on client-side controls (CSS `display: none`, JavaScript filters) to protect restricted data or manage access controls. Premium data or premium articles must be stripped out at the server level unless the user session is fully authenticated and authorized.
- **Keep Production Code Sanitized:** Remove old backup archives (`.zip`, `.bak`, `.tmp`) and testing interfaces from production directories entirely.

---

## 📊 Key Takeaways

- **Client vs Server Trust:** Everything sent to the client (browser) can be viewed, altered, or bypassed by the user. Client-side security controls do not exist.
- **Automated Scanning Limits:** Automated security scripts struggle to interpret human context inside developer notes or parse broken client-side script threads, proving manual testing is mandatory.
- **Hidden Footprints:** Inspecting frameworks, configuration structures, and background API queries (AJAX) reveals massive hidden surfaces that are often left weakly defended compared to main application endpoints.

---

## 🧠 Final Summary

Manually reviewing web applications through built-in browser utilities reveals crucial flaws that automation easily skips. By inspecting static source code comments, exploiting directory misconfigurations, modifying DOM structural attributes via the Element Inspector, freezing client-side runtime libraries via Debugger breakpoints, and tracking background network payloads, penetration testers can thoroughly aud its web applications without relying on external security tools.

---

**Task Status: ✔ Completed**