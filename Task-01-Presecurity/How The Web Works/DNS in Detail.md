# 🌐 Room: DNS in Detail (TryHackMe)

This room focused on understanding how DNS works, how domain names are structured, and how devices communicate over the Internet using human-readable addresses instead of numerical IP addresses.

DNS (**Domain Name System**) acts as the Internet’s phonebook by translating domain names such as `google.com` into IP addresses like `104.26.10.229`. Without DNS, users would need to remember complex numerical addresses for every website they visit.

The room also covered the hierarchy of domain names. The last section of a domain is called the **Top-Level Domain (TLD)**, which can either be a **gTLD** such as `.com` and `.org`, or a **ccTLD** such as `.uk` and `.ir`. Before the TLD comes the **Second-Level Domain (SLD)**, while additional sections placed before it are known as **Subdomains**. Domain names can contain letters, numbers, and hyphens, with a maximum total length of 253 characters.

Several important DNS record types were introduced:
- **A Record** → Maps a domain to an IPv4 address
- **AAAA Record** → Maps a domain to an IPv6 address
- **CNAME Record** → Aliases one domain to another
- **MX Record** → Handles email routing and mail servers
- **TXT Record** → Stores text-based data used for verification and security purposes

The DNS resolution process was explained step-by-step. When a user requests a domain:
1. The computer first checks its local DNS cache.
2. If unavailable, the request is sent to a **Recursive DNS Server** (usually provided by the ISP).
3. The recursive server communicates with the **Root DNS Server**.
4. The root server redirects the request to the correct **TLD Server**.
5. The TLD server points to the **Authoritative DNS Server**.
6. Finally, the authoritative server returns the requested DNS record.

Another important concept was **TTL (Time To Live)**, which determines how long DNS records should remain cached before requiring a new lookup. Caching improves performance and reduces unnecessary DNS traffic.

The room also introduced practical DNS enumeration using the `nslookup` command to retrieve different record types such as A, MX, TXT, and CNAME records.

## 🧠 Key Takeaways
- DNS translates domain names into IP addresses.
- Domain names follow a hierarchical structure (TLD, SLD, Subdomain).
- Recursive DNS servers perform lookups on behalf of clients.
- Authoritative DNS servers store the actual DNS records.
- Different DNS records serve different purposes such as web hosting, email routing, and verification.
- TTL improves speed and reduces network overhead through caching.
- `nslookup` can be used to query DNS records for troubleshooting and analysis.

---

**Task Status:** ✅ Completed