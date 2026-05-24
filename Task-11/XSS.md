# 📜 TryHackMe — Cross-Site Scripting (XSS) Walkthrough

This comprehensive guide covers the fundamental and advanced concepts of Cross-Site Scripting (XSS). It details how attackers bypass the Same-Origin Policy (SOP), the core mechanics of various XSS types, and robust defense lines to protect web applications.

---

## 🔍 Task 1 & 2: Introduction, Terminology & Types

### 📁 The Core Mechanism
Cross-Site Scripting (XSS) occurs when an attacker successfully injects malicious scripts (primarily JavaScript) into a trusted website. This injection allows the attacker to bypass the Same-Origin Policy (SOP)—the browser's security bouncer that prevents scripts on one website from interacting with data from another.

### 💻 Testing Ground: Browser Developer Consoles
To analyze and audit XSS vulnerabilities, penetration testers use the browser's developer tools:
* **Firefox:** `Ctrl + Shift + K`
* **Chrome:** `Ctrl + Shift + J`
* **Safari:** `Command + Option + J`

#### 🛠️ Useful JS Audit Snippets:
* `alert('XSS')` — Basic proof-of-concept pop-up.
* `alert(document.cookie)` — Inspects session cookies available to the DOM.
* `btoa('text')` & `atob('hash')` — Standard Base64 encoding/decoding functions for filter evasion.

### ⚔️ The Terrible Trio of XSS
1.  **Reflected XSS:** A non-persistent attack where the malicious script is part of the request sent to the server (e.g., via a URL parameter) and is immediately "reflected" back in the HTTP response.
2.  **Stored XSS:** A persistent and higher-impact attack where the malicious payload is saved permanently in the target database (e.g., in comment sections, profiles) and executes every time a user visits the corrupted page.
3.  **DOM-Based XSS:** An attack where the vulnerability exists entirely within the client-side JavaScript processing, modifying the page structure directly in the browser session without server-side interaction.

* **[Question 2.1]** Which XSS vulnerability relies on saving the malicious script?
    * **Answer:** `Stored XSS`
* **[Question 2.2]** Which prevalent XSS vulnerability executes within the browser session without being saved?
    * **Answer:** `Reflected XSS`
* **[Question 2.3]** What does DOM stand for?
    * **Answer:** `Document Object Model`

---

## 🏗️ Task 3: Causes and Implications

### 🚨 Why XSS Still Persists
* **Sloppy Input Handling:** Reflecting or storing user input straight into the HTML without sanitization.
* **Lack of Output Encoding:** Failing to convert unsafe characters (like `<`, `>`, `&`) into safe HTML entities.
* **Weak Security Headers:** Poorly configured or missing Content Security Policy (CSP).
* **Vulnerable Third-Party Components:** Utilizing unpatched plugins or libraries.

### 💥 The Fallout of a Breach
* **Session Hijacking:** Stealing session tokens (`document.cookie`) to masquerade as an authenticated user.
* **Phishing & Credential Theft:** Overlaying fake, highly convincing login forms over legitimate pages.
* **Malware Delivery:** Forcing browsers to initiate drive-by downloads.

* **[Question 3.1]** Based on the leading causes of XSS vulnerabilities, what operations should be performed on the user input?
    * **Answer:** `validation and sanitization`
* **[Question 3.2]** To prevent XSS vulnerabilities, what operations should be performed on the data before it is output to the user?
    * **Answer:** `encoding`

---

## 🎣 Task 4 & 5: Reflected XSS & Lab 1

### ☣️ Vulnerable Parameter Demonstration
```http
[http://example.com/search?q=](http://example.com/search?q=)<script>alert(document.cookie)</script>
```
If the backend accepts `q` and renders it blindly, execution triggers immediately.

### 🛡️ Code-Level Remediation Across Languages

* **PHP:** Convert raw strings into safe HTML representations:
    ```php
    echo htmlspecialchars($_GET['q']);
    ```
* **Python (Flask):** Explicitly escape strings when necessary:
    ```python
    from html import escape
    return f"Result: {escape(query)}"
    ```
* **JavaScript (Node.js):** Utilize specialized sanitization libraries:
    ```javascript
    const sanitizeHtml = require('sanitize-html');
    res.send(sanitizeHtml(req.query.q));
    ```

---

### 🔬 Lab 1 Walkthrough (Copyparty Exploitation)
* **Target Vulnerability:** Copyparty File-Sharing Server (`CVE-2023-38501`) on port `3923`.
* **Vector Mechanism:** Injecting an image asset containing a faulty source and an active execution hook: `<img src=x onerror=alert(1)>`.

* **[Question 5.1]** What type of vulnerability is it?
    * **Answer:** `Reflected XSS`
* **[Question 5.2]** Use the above exploit against the attached VM. What do you see on the second line after go to?
    * **Answer:** `/?h#cc`
* **[Question 4.1]** Which one of the following characters do you expect to be encoded? `.`, `,`, `;`, `&`, or `#`?
    * **Answer:** `&`
* **[Question 4.2]** Which one of the following characters do you expect to be encoded? `+`, `-`, `*`, `<`, `=`, or `^`?
    * **Answer:** `<`
* **[Question 4.3]** Which function can we use in JavaScript to replace (unsafe) special characters with HTML entities?
    * **Answer:** `escapeHtml()`
* **[Question 4.4]** Which function did we use in PHP to replace HTML special characters?
    * **Answer:** `htmlspecialchars()`

---

## 💾 Task 6 & 7: Stored XSS & Lab 2

### 💉 Vulnerable Flow Analysis
When input is inserted directly into query executions, it sits in the database until retrieved and rendered flat onto a victim's screen:

```php
// VULNERABLE RENDERING SOURCE
while ($row = mysqli_fetch_assoc($result)) {
    echo $row['comment']; // Malicious payload triggers here for every visitor
}
```

### 🔬 Lab 2 Walkthrough (Hospital Management System)
* **Target Vulnerability:** Vintage HMS Project (`CVE-2021-38757`) running on port `80`.
* **Exploit Vector:** Dropping `<script>alert(document.cookie)</script>` directly into the Contact form message field (`contact.php`).
* **Trigger Hook:** Log into the administrative dashboard using `admin:admin123`. The stored script pops up immediately upon loading the receptionist view.

* **[Question 7.1]** What type of vulnerability is it?
    * **Answer:** `Stored XSS`
* **[Question 7.2]** Go to the contact page and submit the payload. Log in as Receptionist. What is the name of the key from the displayed key-value pair?
    * **Answer:** `PHPSESSID`
* **[Question 6.1]** What is the name of the JavaScript function we used to sanitize the user input before saving it?
    * **Answer:** `sanitizeHTML()`
* **[Question 6.2]** Which method did we call in ASP.Net C# to sanitize user input?
    * **Answer:** `HttpUtility.HtmlEncode()`

---

## 🌳 Task 8: DOM-Based XSS

### 🪓 The Client-Side Trap
DOM-XSS happens entirely inside the victim's browser session. For example, using a highly dangerous source/sink combo like `document.write()`:

```javascript
// HIGHLY VULNERABLE CODE
const name = new URLSearchParams(window.location.search).get('name');
document.write("Hello, " + name); 
```
If an attacker passes `?name=<script>alert(1)</script>`, client-side execution fires off without the payload ever reaching the backend web server logs.

### 🛡️ Secure DOM Modification
```javascript
// SECURED CODING PRACTICE
document.getElementById("greeting").textContent = "Hello, " + escapedName;
```

* **[Question 8.1]** DOM-based XSS is reflected via the server. (Yea/Nay)
    * **Answer:** `Nay`
* **[Question 8.2]** DOM-based XSS happens only on the client side. (Yea/Nay)
    * **Answer:** `Yea`
* **[Question 8.3]** Which JavaScript method was used to escape the user input?
    * **Answer:** `encodeURIComponent()`

---

## 🎭 Task 9 & 10: Context, Evasion & Conclusion

### 🎯 Identifying Execution Contexts
* **Between HTML Tags:** Direct execution (e.g., `<script>...</script>`).
* **Within HTML Attributes:** Requires breaking out of quotes (e.g., `"> <script>...`).
* **Inside Existing JavaScript Blocks:** Breaking string contexts and handling trailing code (e.g., `'; alert(1); //`).

### 🥷 Common Filter Evasion Gymnastics
When simple string matches block standard words or spaces, attackers leverage alternative whitespace hex values like Horizontal Tabs (`&#x09;`), New Lines (`&#x0A;`), or Carriage Returns (`&#x0D;`) inside an asset tag source:

```html
<IMG SRC="jav&#x09;ascript:alert('XSS');">
```

* **[Question 9.1]** Which character does &#x09; represent?
    * **Answer:** `Tab`
* **[Question 10.1]** This room used a fictional static site to demonstrate one of the XSS vulnerabilities. Which XSS type was that?
    * **Answer:** `DOM-based XSS`

---
* **Task Status:** ✔ Room Completed Successfully