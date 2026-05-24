# 🗺️ Server-Side Request Forgery (SSRF) — Complete Reference Guide

Server-Side Request Forgery (SSRF) is a critical web security vulnerability that allows an attacker to induce the server-side application to make HTTP requests to an unintended location.

---

# 💥 Impact & Attack Vectors

An attacker can weaponize an SSRF vulnerability to abuse the server's local network trust relationships, pivoting attacks inward toward internal infrastructure or outward toward third-party systems.

## 1. Attacks Against the Local Server (Loopback)

Attackers modify target parameters to force the server to loop back to its own local interface using hostnames like `127.0.0.1` or `localhost`.

### The Mechanism

Access control checks are often implemented in a component sitting *in front* of the application server (like a reverse proxy or API gateway). When a connection bypasses this component and originates directly from the server itself via the loopback interface, access controls are frequently bypassed, granting unauthenticated administrative access.

### Example Application Traffic

```http
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=6&storeId=1
```

### Malicious Exploit Payload

```http
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://localhost/admin
```

---

## 2. Attacks Against Other Back-End Systems

In many cases, the application server can interact with back-end systems that are not directly reachable by users. These internal back-end systems reside behind the firewall within a trusted network topology, use non-routable private IP addresses (e.g., `192.168.0.68`), and often feature a weaker security posture lacking robust authentication.

### Malicious Exploit Payload

```http
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://192.168.0.68/admin
```

---

# 🔀 Circumventing SSRF Defenses

Applications often deploy input filters to intercept malicious payloads. These normalization layers and cookies can frequently be bypassed using logical structural tricks.

---

## 1. SSRF with Blacklist-Based Input Filters

When an application explicitly blocks inputs containing strings like `127.0.0.1`, `localhost`, or `/admin`:

### Alternative IP Representations

Translate the target IP into alternative structural formats:

- Decimal: `2130706433`
- Octal: `017700000001`
- Alternative Range: `127.1` or `127.0.0.2`

### Custom DNS Pointer

Register a personal domain name that resolves via an A record directly to `127.0.0.1` (or leverage pre-configured endpoints like `spoofed.burpcollaborator.net`).

### String Obfuscation

Apply URL encoding, double encoding, or switch alphabetical case configurations to slip past naive regex configurations.

### Open Redirection Pivoting

If the filter strictly validates the initial domain but allows a trusted partner site containing an open redirect vulnerability, use that redirector to forward the backend request to your internal target.

```http
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://weliketoshop.net/product/nextProduct?currentProductId=6&path=http://192.168.0.68/admin
```

---

## 2. SSRF with Whitelist-Based Input Filters

When an application restricts input exclusively to a rigid array of allowed domains, exploit structural inconsistencies in ad-hoc URL parsers.

### Credential Embedding (`@`)

```text
https://expected-host:fakepassword@evil-host
```

### Fragment Indicators (`#`)

```text
https://evil-host#expected-host
```

### DNS Hierarchy Exploitation

```text
https://expected-host.evil-host
```

### URL Encoding Discrepancies

Encode or double-encode the `#` or `@` characters. If the input filter decodes the URL differently than the backend HTTP client component performing the request, validation checks can be completely blinded.

---

# 👁️ Blind SSRF Vulnerabilities

A Blind SSRF occurs when you can force the application to issue an outbound back-end HTTP request to a supplied URL, but the response from that request is not returned in the application's front-end response.

---

## Discovery & Detection

The most definitive mechanism to map blind attack vectors is via Out-of-Band (OAST) application security testing tools like Burp Collaborator.

### The Method

Generate a unique domain name using Burp Collaborator, send this domain in payloads to the application, and monitor for incoming network interactions.

### ⚠️ Note on DNS vs. HTTP Logs

It is common when testing for SSRF to observe a DNS look-up for the supplied Collaborator domain without receiving a subsequent HTTP connection. This indicates that network-level egress firewalls are actively blocking foreign outbound HTTP traffic, while infrastructure DNS traffic remains open.

---

# 🚀 Advanced Blind Exploitation

## Internal Network Sweeping

Because you cannot view responses directly, use blind scripts to sweep internal subnets. Send targeted exploits (e.g., payloads weaponized against known vulnerabilities like Shellshock) to internal IP paths while monitoring for OAST callouts from hit targets.

## Exploiting the Internal HTTP Client

Induce the server's internal HTTP fetching library to connect back to an attacker-controlled server. By returning highly specific, malicious exploit chunks targeting vulnerabilities within the server's own client runtime library, you can achieve remote code execution (RCE).

---

# 🔎 Finding Hidden Attack Surfaces

## Partial URLs in Requests

Look closely at fields where you supply a single directory node or partial path string that the backend concatenates into an internal resource locator request.

## URLs Within Data Formats (XML / XXE)

Check application endpoints that ingest structured file formats. Parsing raw XML data can open avenues for XML External Entity (XXE) injections, which natively integrate SSRF behavior via external entity definitions.

## The Referer Header

Web analytics software regularly logs incoming connection pathways by picking up the client `Referer` header to track incoming links. Some analytics packages automatically perform asynchronous HTTP fetch cycles back to these logged URLs to parse the referring sites, exposing an unexposed SSRF attack surface.

---

# ✅ Task Status ✔ Completed