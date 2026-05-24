# 🗺️ TryHackMe — Content Discovery Walkthrough (Summary)

This module covers the "Content Discovery" room on TryHackMe, which focuses on exploring and mapping hidden or private assets on a web server. Discovering unlinked or unintended files, backups, administration panels, and configuration fragments often uncovers critical vulnerabilities.

---

## 🔍 Core Methodologies of Content Discovery

Web content discovery is systematically divided into three fundamental execution models:

1. **Manual Discovery:** Leveraging human intuition to inspect public files, standard web files, source headers, and framework configurations.
2. **Automated Discovery:** Utilizing fuzzing utilities and extensive wordlists to systematically guess hundreds of thousands of path combinations.
3. **OSINT (Open-Source Intelligence):** Aggregating historical, organizational, and technological footprints from third-party public repositories and search indexes.

---

## 🛠️ Deep Dive: Manual Discovery Tactics

### 1. Web Crawling Controls (`robots.txt`)

- **Purpose:** A text file instructing search engine spiders which paths they are restricted from indexing.
- **Security Insight:** Security engineers often place sensitive directories (e.g., login portals, staging links) inside this file, unintentionally turning it into a roadmap for penetration testers.
- **Target Vector Checked:** `/staff-portal` (Discovered via `http://<MACHINE_IP>/robots.txt`).

---

### 2. Application Footprinting via Favicon

- **Purpose:** Analyzing the small branding icon in the browser tab to identify the underlying web stack.
- **Execution:** When developers fail to replace default framework icons, testers can compute the file's MD5 cryptographic hash and cross-reference it with the **OWASP Favicon Database**.

#### Practical Check

```bash
curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico | md5sum
```

- **Discovered Stack:** The unique hash mapped directly to the `cgiirc` framework.

---

### 3. Search Index Maps (`sitemap.xml`)

- **Purpose:** An XML document explicitly listing all active endpoints that the site owner wants search engines to find.
- **Security Insight:** It frequently exposes orphaned legacy links, hidden rich features, or deep application paths that lack direct UI anchors.
- **Target Vector Checked:** `/s3cr3t-area` (Discovered via `http://<MACHINE_IP>/sitemap.xml`).

---

### 4. Intercepting Response Headers

- **Purpose:** Capturing key/value pairs returned in the HTTP response body to map backend platforms and software versions.
- **Execution:** Running a verbose cURL session against the target server to dump raw response metadata.

```bash
curl http://<MACHINE_IP> -v
```

- **Discovered Leak:** Exposed server application architecture (e.g., Nginx, PHP versions) and a hidden security flag value:
  
```text
THM{HEADER_FLAG}
```

inside the custom `X-FLAG` header field.

---

### 5. Reviewing Framework Stacks

- **Purpose:** Reading structural clues like page load generation timestamps, copyright parameters, or framework credits inside source code comments.
- **Execution:** Following framework links to public documentation portals to locate standard, unedited path maps.
- **Target Vector Checked:** Accessing the default administration path leaked by the framework site led to:

```text
THM{CHANGE_DEFAULT_CREDENTIALS}
```

---

## 🌐 Open-Source Intelligence (OSINT) Footprinting

When direct interaction with the host is limited, external platforms provide extensive historical and technical telemetry:

| OSINT Vector / Tool | Core Technical Utility | Key Query / Format Syntax |
| :--- | :--- | :--- |
| **Google Dorking** | Employs advanced search operators to isolate specific subdomains, exposed endpoints, or files indexed by Google. | `site:<target.com> admin` |
| **Wappalyzer** | Fingerprints frameworks, CMS platforms, payment processors, and backend technologies. | Extension Profile Scan |
| **Wayback Machine** | Historical archive exposing deprecated pages and legacy resources. | `https://archive.org/web/` |
| **GitHub Recon** | Searches public repositories for leaked source code, credentials, or configuration files. | Version Control Search |
| **Amazon S3 Buckets** | Investigates exposed cloud storage buckets with improper ACL configurations. | `http(s)://{name}.s3.amazonaws.com` |

---

## 🤖 Automated Path Discovery & Directory Fuzzing

Automated content discovery relies on sending high volumes of HTTP requests against a target server to determine whether files or directories exist. These attacks rely heavily on curated **wordlists** such as **SecLists**.

---

## ⚙️ Automation Execution Profiles

The room demonstrates three popular fuzzing and enumeration utilities:

### 1. FFUF (Fuzz Faster U Fool)

```bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://10.10.28.226/FUZZ
```

---

### 2. DIRB (Directory Bruteforcer)

```bash
dirb http://10.10.28.226/ /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
```

---

### 3. Gobuster (High-Speed Enumerator)

```bash
gobuster dir --url http://10.10.28.226/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
```

---

## 📋 Automation Output Analysis

The automated enumeration engines successfully uncovered hidden application resources:

| Discovered Resource | Security Impact |
| :--- | :--- |
| `/monthly` | Hidden application directory |
| `/development.log` | Sensitive development log exposure |

---

## 🛡️ Remediation & Defensive Security Baselines

To mitigate risks associated with content discovery and information leakage:

- **Enforce Strict Cloud ACLs:** Audit Amazon S3 bucket permissions dynamically and require authenticated IAM-based access.
- **Sanitize Public Files:** Never expose sensitive paths inside `robots.txt` or `sitemap.xml`.
- **Suppress Server Banners:** Remove identifying server signatures such as `Server:` and `X-Powered-By` headers.
- **Production Asset Cleaning:** Strip developer comments, remove backup files (`.zip`, `.bak`, `.tmp`), and securely dispose of old logs.
- **Rigorous Access Controls:** Hidden endpoints must still enforce proper server-side authentication and authorization.

---

## 📊 Key Takeaways

- Hidden content frequently exposes the most valuable attack surface during web application assessments.
- `robots.txt` and `sitemap.xml` files commonly leak sensitive application structure.
- Framework fingerprinting dramatically improves reconnaissance efficiency.
- OSINT intelligence sources can reveal historical or external attack vectors.
- Automated fuzzing tools rapidly expand visibility into hidden infrastructure.
- Security through obscurity is not a valid defensive strategy.

---

## 🧠 Final Summary

The Content Discovery room demonstrates how penetration testers systematically uncover hidden web application resources through manual analysis, OSINT intelligence gathering, and automated directory fuzzing techniques.

By reviewing robots.txt files, sitemap indexes, favicon hashes, framework documentation, HTTP response headers, cloud storage patterns, and public repositories, attackers can map hidden infrastructure and identify sensitive resources unintentionally exposed by developers.

The module highlights the importance of combining human intuition with automated enumeration tools such as FFUF, Gobuster, and DIRB to comprehensively explore an application's hidden attack surface.

---

**Task Status: ✔ Completed**