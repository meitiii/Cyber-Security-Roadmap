# 🛡️ SQL Injection (SQLi) — Web Security Academy (Summary)

This module explains SQL Injection (SQLi), a critical web security vulnerability that allows attackers to interfere with database queries. It details how to identify, exploit, and prevent these vulnerabilities, which can lead to unauthorized data access, modification, or total server compromise.

---

## 🔍 Understanding SQL Injection

SQLi occurs when an application incorporates untrusted user input into database queries in an unsafe way.

- **Core Mechanism:** Attackers inject malicious SQL syntax to manipulate the structure of the database query.
- **Potential Impact:**
    - **Unauthorized Access:** Retrieving sensitive data (passwords, credit card details, user info).
    - **Data Manipulation:** Modifying or deleting data, or causing persistent changes to application behavior.
    - **Full Compromise:** Escalating access to the underlying server or infrastructure.
    - **Availability:** Performing denial-of-service (DoS) attacks.

---

## 🛠️ Common Injection Techniques

### 1. Retrieving Hidden Data
By injecting Boolean logic (e.g., `' OR 1=1--`), an attacker can force a query to return all records, bypassing filters like `WHERE released = 1`.

### 2. Subverting Application Logic
Attackers can bypass authentication by using comments (e.g., `--`) to neutralize password checks in login queries.

### 3. UNION Attacks
If an application reflects query results, an attacker can use the `UNION` keyword to append results from unauthorized database tables (e.g., dumping the `users` table).

### 4. Blind SQL Injection
When the application does not return query results or errors, attackers can use:
- **Boolean-based:** Triggering detectable changes in application responses based on true/false conditions.
- **Time-based:** Inducing delays to infer data based on response times.
- **Out-of-Band (OAST):** Forcing the server to trigger a network interaction (e.g., DNS lookup) to an attacker-controlled domain.

---

## 📋 Identifying & Examining the Database

- **Detection:** Systematic testing using characters like `'`, Boolean conditions (`OR 1=1`), time delays, or Burp Scanner.
- **Database Profiling:** Once a vulnerability is found, testers obtain database metadata (version, table names, column structure) using platform-specific queries (e.g., `information_schema.tables`).
- **Context Awareness:** SQLi can occur in any query part (SELECT, UPDATE, INSERT, ORDER BY) and in various formats (JSON, XML), requiring different encoding/obfuscation techniques to bypass filters.

---

## 🛡️ Prevention Strategies

The most effective defense against SQL injection is the use of **parameterized queries** (prepared statements).

- **How it works:** Instead of concatenating input, you define the query structure and pass user input as separate parameters. This ensures the database engine treats input strictly as data, not executable code.
- **Limitations:** Parameterized queries cannot handle dynamic table/column names. In these cases, implement strict **whitelisting** of permitted values or use alternative logic.
- **Critical Rule:** The SQL query string must always be a hard-coded constant; never use string concatenation for any part of the query that includes user-provided data.

---

## 📊 Key Takeaways

- **Stop Concatenation:** Never concatenate user input directly into SQL strings.
- **Database Structure:** Understand that injection is not limited to `WHERE` clauses; it can occur anywhere the application handles data.
- **Defense-in-Depth:** While prepared statements are the gold standard, employ automated scanning tools to catch vulnerabilities during development.
- **Second-Order SQLi:** Be aware that stored data (e.g., user profiles) can be malicious and trigger injection later when retrieved by the application.

---

## 🧠 Final Summary

SQL injection remains one of the most impactful vulnerabilities in web security. By understanding the query structure, leveraging systematic detection methods, and strictly implementing parameterized queries, developers can effectively eliminate this risk and protect sensitive organizational data.

---

**Task Status: ✔ Completed**