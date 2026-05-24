# 🌐 TryHackMe — Subdomain Enumeration Walkthrough (Summary)

This module covers the "Subdomain Enumeration" room on TryHackMe, focusing on expanding a target organization's attack surface by discovering hidden, forgotten, or internally exposed subdomains.

Subdomain enumeration is a critical reconnaissance process because exposed staging systems, legacy services, administration portals, and development environments frequently operate with weaker security controls than primary production assets.

---

## 🔍 Core Methodologies of Subdomain Enumeration

Subdomain discovery is divided into three primary reconnaissance strategies:

| Methodology | Purpose |
| :--- | :--- |
| OSINT (Open-Source Intelligence) | Gathering publicly exposed infrastructure data |
| DNS Brute Forcing | Automating DNS record discovery using wordlists |
| Virtual Host Enumeration | Discovering hidden internal sites through HTTP Host header manipulation |

---

## 🧭 Why Subdomain Enumeration Matters

Enumerating subdomains helps attackers uncover:

- Development environments
- Staging servers
- Administrative portals
- Legacy infrastructure
- Misconfigured cloud assets
- Internal applications exposed externally

These assets often:
- Bypass enterprise monitoring
- Run outdated software
- Reuse weak credentials
- Lack hardened configurations

---

# 🛠️ Deep Dive: Enumeration Techniques & Tasks

---

# 🌐 OSINT — Certificate Transparency (CT) Logs

## 🔎 Technical Background

Whenever a Certificate Authority (CA) issues an SSL/TLS certificate, the issuance event is publicly recorded inside Certificate Transparency (CT) logs.

These logs were designed to:
- Detect rogue certificate issuance
- Improve certificate ecosystem transparency
- Allow organizations to monitor unauthorized certificates

However, they also expose organizational infrastructure details.

---

## ⚠️ Security Insight

Each certificate may reveal:
- Internal subdomains
- Staging environments
- Historical infrastructure
- Deprecated systems

Attackers frequently query CT databases to build target infrastructure maps.

---

## 🔬 Enumeration Technique

Platforms such as:
- `crt.sh`
- Entrust CT Search

can be queried for wildcard and historical certificate records.

---

## 🎯 Target Vector Discovered

```text
store.tryhackme.com
```

The subdomain was recovered through CT log analysis tied to historical certificate issuance records.

---

# 🔎 OSINT — Google Dorking

## 🔬 Technique Overview

Google indexing can expose:
- Hidden subdomains
- Cached assets
- Legacy pages
- Forgotten administrative panels

Advanced search operators allow attackers to isolate specific infrastructure components.

---

## ⚙️ Enumeration Syntax

```text
-site:www.tryhackme.com site:*.tryhackme.com
```

---

## ⚡ Logic Breakdown

| Operator | Function |
| :--- | :--- |
| `site:` | Restrict results to a target domain |
| `-site:` | Exclude specific subdomains |

This strips away the primary site and isolates alternate subdomains.

---

## 🎯 Target Vector Discovered

```text
blog.tryhackme.com
```

---

# 🤖 Automated DNS Brute Force Enumeration

## 🔎 Technical Overview

DNS brute-forcing automates subdomain discovery by testing large wordlists against public DNS resolvers.

The process checks whether candidate hostnames successfully resolve to valid IP addresses.

---

## ⚙️ Common Tooling

| Tool | Purpose |
| :--- | :--- |
| `dnsrecon` | DNS enumeration & reconnaissance |
| `Sublist3r` | Automated subdomain discovery |
| `ffuf` | High-speed fuzzing engine |
| `SecLists` | Large reconnaissance wordlists |

---

## 🎯 Target Vector Checked

```text
web55.acmeitsupport.thm
```

---

# 🖥️ Virtual Host (VHost) Enumeration

## 🔎 Technical Background

Some web applications:
- Are not published in public DNS
- Exist only internally
- Depend entirely on the HTTP `Host:` header

The backend web server uses this header to determine which internal virtual host configuration to serve.

---

## ⚠️ Security Insight

Even when DNS records do not exist publicly, hidden virtual hosts may still respond if the correct `Host:` value is supplied manually.

---

## 🚀 VHost Fuzzing via FFUF

```bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://<MACHINE_IP>
```

---

## ⚙️ Attack Logic

Hydra injects each wordlist value into:

```text
Host: FUZZ.acmeitsupport.thm
```

and observes:
- Response sizes
- Status codes
- Redirect behavior

to identify valid virtual hosts.

---

# 📏 Response Size Filtering (`-fs`)

## 🔎 Problem

Most invalid hostnames return:
- Generic landing pages
- Default 404 responses
- Identical response sizes

This creates noisy output.

---

## ⚙️ Solution

Filter repeated response sizes using:

```bash
-fs <size>
```

---

## 🚀 Optimized Scan

```bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://<MACHINE_IP> -fs <size_value>
```

---

## 🎯 Hidden Virtual Hosts Discovered

| Virtual Host |
| :--- |
| `delta` |
| `yellow` |

---

## ⚠️ Security Impact of Subdomain Exposure

Successful subdomain enumeration may expose:

- Internal administration portals
- Development applications
- Unpatched legacy systems
- Misconfigured staging servers
- Internal APIs
- Sensitive testing infrastructure

---

## 🛡️ Remediation & Defensive Security Baselines

To reduce infrastructure exposure through subdomain leakage:

---

### Use Wildcard Certificates Carefully

Wildcard certificates:

```text
*.domain.com
```

reduce the number of unique CT log entries tied to individual subdomains.

---

### Harden Default Virtual Hosts

Configure catch-all virtual hosts to:
- Reject invalid `Host:` headers
- Drop connections
- Return blank responses

Avoid leaking:
- Framework metadata
- Internal paths
- Technology fingerprints

---

### Implement Split-Horizon DNS

Maintain separate DNS infrastructures for:
- Internal corporate services
- Public-facing infrastructure

Development and administrative systems should never exist in public DNS zones.

---

### Restrict DNS Zone Transfers (AXFR)

Improperly configured name servers may allow attackers to dump entire DNS zone records.

Restrict:
- Unauthorized AXFR requests
- External recursive queries

to trusted internal systems only.

---

## 📊 Key Takeaways

- Subdomain enumeration significantly expands the visible attack surface.
- Certificate Transparency logs leak valuable infrastructure intelligence.
- Google indexing frequently exposes forgotten assets.
- DNS brute-forcing remains highly effective against weak naming schemes.
- Virtual host fuzzing bypasses public DNS limitations entirely.
- Internal systems frequently operate with weaker security controls than production environments.

---

## 🧠 Final Summary

The Subdomain Enumeration room demonstrates how attackers systematically uncover hidden infrastructure through a combination of OSINT collection, DNS brute-forcing, and virtual host fuzzing techniques.

By analyzing Certificate Transparency logs, leveraging advanced Google search operators, automating DNS discovery, and manipulating HTTP Host headers, penetration testers can identify undocumented systems, staging environments, and internal applications that significantly expand an organization's attack surface.

The module reinforces the importance of hardened DNS configurations, proper infrastructure segmentation, secure virtual host management, and minimizing publicly exposed organizational metadata.

---

**Task Status: ✔ Completed**