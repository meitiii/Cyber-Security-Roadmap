# 🧃 OWASP Juice Shop — SQL Injection Challenges

These challenges demonstrate how improper input sanitization in login fields allows attackers to bypass authentication by manipulating backend SQL queries.

---

## 🔑 Login Bypasses

*   **Administrator Account:**
    *   **Payload:** `' OR 1=1--`
    *   **Concept:** By injecting `OR 1=1`, the query condition becomes universally true, forcing the database to return the first user record (typically the Administrator).
*   **Bender Account:**
    *   **Payload:** `bender@juice-sh.op'--`
    *   **Concept:** By specifying the valid email and commenting out the password verification logic with `--`, the application authenticates the session as that specific user.

---

## 🛡️ Engineering Insight
These exploits underscore the danger of "Insecure Design" and "Injection" in the OWASP Top 10. Even in a modern Node.js/Express environment, if developer code concatenates user input directly into database queries (e.g., `SELECT * FROM Users WHERE email = '` + input + `'`), no amount of network-level security can prevent the compromise. Always use **parameterized queries** (prepared statements) to ensure the database treats input strictly as data, never as executable code.

---

**Task Status: ✔ Completed**