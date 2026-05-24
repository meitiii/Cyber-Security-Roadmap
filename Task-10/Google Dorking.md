# 🔍 TryHackMe — Google Dorking Walkthrough (Summary)

This module covers the "Google Dorking" room on TryHackMe, which focuses on utilizing advanced search engine operators to filter public indexes, perform passive reconnaissance, and discover exposed sensitive assets or misconfigured directories.

---

## 🕷️ Core Concepts: Web Crawlers & Indexing

Before diving into Dorking, it is essential to understand how search engines collect and map information across the internet:

- **Crawling:** The automated process where search engine bots (spiders/clippers) systematically follow links across the web to fetch page data.
- **Index:** A structured, massive database collection of web resources and their precise locations compiled by crawlers.
- **Keywords:** Core textual elements, headings, and metadata gathered from web pages that engines use to understand, categorize, and rank content.

---

## 🤖 Web Crawling Controls & Architecture

Websites use specific root-level files to guide or restrict compliant search engine spiders.

### 1. The `robots.txt` Protocol
A voluntary text-based configuration file always located at the absolute root of a domain (e.g., `domain.com/robots.txt`).

- **User-agent:** Directs rules to a specific crawler (e.g., `User-agent: Bingbot` flags only Microsoft's crawler).
- **Disallow:** Tells compliant bots which directory paths or routes to ignore (e.g., `Disallow: /dont-index-me/`).
- **High-Risk File Extensions:** System configuration fragments like `.conf` files should always be hidden from public exposure and secured at the server level, as they frequently leak internal environmental variables.

### 2. Search Index Sitemaps (`sitemap.xml`)
- **Structure:** An **XML** formatted document designed to act as a literal "map" of a website's structural architecture.
- **Utility:** It directly details the explicit **routes** (the paths taken for content following the main domain) that owners actively want search engines to discover and list cleanly.

---

## 🎯 Advanced Google Search Operators (Google Dorking)

Google Dorking involves combining advanced filters to strip out generic search results and pinpoint specific exposures.

| Operator | Core Technical Function | Practical Example Syntax |
| :--- | :--- | :--- |
| `site:` | Limits search results strictly to a specific domain or TLD. | `site:bbc.co.uk flood defences` |
| `filetype:` | Filters results to locate specific file extensions (e.g., PDF, DOCX, log, conf). | `site:gov.uk filetype:pdf "flood defences"` |
| `intitle:` | Searches for pages containing specific keywords within their HTML `<title>` tags. | `intitle:"login" site:example.com` |

---

## 🛡️ Remediation & Defensive Security Baselines

To safeguard web infrastructure against Google Dorking and passive asset exposure:

- **Robots.txt is NOT a Security Boundary:** The `robots.txt` file is completely voluntary; malicious actors or custom scrapers will aggressively ignore `Disallow` rules. Never rely on it to hide sensitive data.
- **Server-Level Access Restrictions:** Remove sensitive files (`.bak`, `.log`, `.conf`) from public web directories completely, or implement server-side access controls (such as `.htaccess` restrictions, IP whitelisting, or IAM authentication frameworks).
- **Enforce Custom Indexing Directives:** Use the `X-Robots-Tag` HTTP header or `<meta name="robots" content="noindex">` directly inside HTML headers to firmly prevent pages from being cached or indexed by search engines.
- **Coordinated Disclosure:** If exposed Personally Identifiable Information (PII) or security holes are found via passive dorking, follow ethical standards and report the findings directly to the platform owners via an authorized disclosure path.

---

**Task Status: ✔ Completed**