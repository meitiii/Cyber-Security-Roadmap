# 🛡️ Advanced SQL Injection (UNION & Blind) — Web Security Academy (Summary)

This guide covers advanced exploitation techniques for SQL injection, focusing on cases where results are directly visible (UNION-based) or hidden (Blind SQLi).

---

## 🔗 UNION-Based SQL Injection

When query results are visible in the application response, `UNION` attacks allow retrieving data from other tables.

### Requirements
1.  **Column Count Match:** Both original and injected queries must return the same number of columns.
    *   *Method:* Use `ORDER BY [index]--` or `UNION SELECT NULL, NULL...--` to find the exact count.
2.  **Compatible Data Types:** Injected columns must support the same data types as the original query.
    *   *Method:* Probe columns by injecting string values (`'a'`) one by one.

### Retrieving Data
*   **Concatenation:** If only one column is available, use concatenation operators (e.g., `||` on Oracle, `CONCAT()` on others) to combine multiple values (e.g., `username` and `password`) into a single output field.

---

## 👁️ Blind SQL Injection

Occurs when the application does *not* return database results or errors. Exploitation relies on inferring data through application behavior.

### 1. Conditional Responses
Inject conditions (e.g., `AND 1=1`) and observe if the application's response (e.g., "Welcome back") changes. By systematically testing conditions character-by-character (e.g., using `SUBSTRING`), you can extract entire data strings.

### 2. Error-Based SQLi
*   **Triggering Errors:** Induce database errors (e.g., divide-by-zero) conditionally. If the error occurs only when your condition is true, you can infer the data.
*   **Verbose Errors:** Sometimes, misconfigured databases output raw query data in error messages. Use functions like `CAST()` to force conversion errors that reveal query contents.

### 3. Time-Based SQLi
If responses are identical regardless of conditions, use time-delay commands (e.g., `WAITFOR DELAY` on MSSQL, `pg_sleep()` on PostgreSQL) triggered by conditional statements to measure response latency.

### 4. Out-of-Band (OAST)
The most reliable method for asynchronous applications.
*   **Mechanism:** Force the database to perform an external network request (e.g., a DNS lookup) to an attacker-controlled server (e.g., **Burp Collaborator**).
*   **Exfiltration:** Embed the extracted data (e.g., a password) as a subdomain in the DNS query to exfiltrate it directly through the OAST channel.

---

## 📋 Database Enumeration

Identifying the database structure is critical for successful exploitation:

*   **Version/Type:** Use provider-specific queries (e.g., `@@version` for MSSQL/MySQL, `version()` for PostgreSQL, `v$version` for Oracle).
*   **Schema Discovery:**
    *   **Non-Oracle:** Query `information_schema.tables` and `information_schema.columns`.
    *   **Oracle:** Query `all_tables` and `all_tab_columns`.

---

## 📊 Key Takeaways

*   **Systematic Testing:** Use `ORDER BY` to find column counts and Burp Scanner to detect subtle anomalies.
*   **Database Syntax:** Always refer to SQL cheat sheets, as syntax for comments (`--`, `#`), concatenation (`||`), and string manipulation (`SUBSTR`, `SUBSTRING`) varies by platform.
*   **Prevention:** Blind and UNION-based attacks are stopped by the same defense as regular SQLi: **Parameterized Queries (Prepared Statements)**.
*   **Defense-in-Depth:** If parameterized queries aren't possible (e.g., dynamic table names), implement strict **whitelisting** for all user-provided inputs.

---

## 🧠 Final Summary

Advanced SQLi requires moving beyond simple syntax errors to logical inference. Whether using visible UNION results or sophisticated out-of-band network triggers, the core goal remains the same: manipulating the database's execution logic. Proper implementation of prepared statements remains the industry-standard defense against all forms of SQLi.

---

**Task Status: ✔ Completed**