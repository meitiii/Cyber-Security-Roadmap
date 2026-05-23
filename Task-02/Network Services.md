# 💼 Network Services — TryHackMe (Summary)

This room explores the enumeration and exploitation of three core network services: SMB, Telnet, and FTP. It provides hands-on experience with identifying misconfigurations, leveraging anonymous access, generating reverse shells, and performing password brute-forcing to compromise target machines.

---

## 🌐 SMB (Server Message Block)
SMB is a response-request protocol used for sharing files, printers, and resources on a network, typically operating on TCP ports 139 and 445.

- **Enumeration:** Using `enum4linux`, you can extract workgroup names, operating system versions, user lists, and available shares (e.g., discovering a `profiles` share).
- **Exploitation:** Misconfigured shares often allow anonymous access. Using `smbclient`, attackers can log in without a password, traverse directories, and download sensitive files (such as an `.ssh/id_rsa` private key) to gain unauthorized SSH access to the system.

---

## 🛠️ Telnet & Reverse Shells
Telnet is a legacy application protocol that transmits data in plaintext (lacking encryption) and has largely been replaced by SSH.

- **Hidden Services:** Telnet isn't always on its default port (23). In this scenario, a Telnet backdoor was discovered on an unassigned port (8012) by running an all-ports Nmap scan (`-p-`).
- **Exploitation:** After verifying the ability to execute system commands (by sending ICMP pings and monitoring with `tcpdump`), a reverse shell payload was generated using `msfvenom` (utilizing a `mkfifo` netcat payload). A Netcat listener (`nc -lvp 4444`) was set up locally to catch the incoming connection and grant a remote shell.

---

## 📂 FTP (File Transfer Protocol)
FTP uses a client-server model for transferring files and operates by default on port 21.

- **Information Gathering:** Logging into the `vsftpd` server using the `anonymous` account revealed a public text file, which inadvertently leaked a valid username (`mike`).
- **Brute-forcing:** Armed with a valid username, the `hydra` tool was deployed alongside the `rockyou.txt` wordlist to brute-force the password, successfully granting authenticated access to the server.

---

## 🧠 Career Direction Insight
This room perfectly bridges the gap between theoretical network engineering and practical offensive security.

- **Current Learning Focus:** Service enumeration, exploiting access control misconfigurations, payload generation, and online dictionary attacks.
- **Application:** For someone with a solid foundation in Cisco networking and infrastructure engineering, understanding how application-layer protocols are exploited adds critical depth. It demonstrates exactly why secure configurations—such as disabling anonymous SMB/FTP logins, deprecating Telnet, and enforcing strict firewall rules—are paramount for both SOC analysts defending a network and Penetration Testers auditing it.

---

## 📊 Key Takeaways
- **Anonymous Access:** Always check for anonymous logins on SMB and FTP; it is a prevalent misconfiguration that often leads to total system compromise.
- **Enum4linux:** An essential, time-saving wrapper tool for extracting domains, users, and shares from Windows and Samba servers.
- **Non-Standard Ports:** Backdoors and vulnerable services are frequently hidden on high-range, non-standard ports to evade default Nmap scans.
- **Hydra & Wordlists:** Bruteforcing is highly effective when prior enumeration has successfully yielded a valid username.
- **msfvenom:** A vital framework tool for quickly generating customized payloads and reverse shells tailored to specific target environments.

---

## 🧠 Final Summary
Network services act as the primary gateways into any system. While robust infrastructure and routing are essential, securing the specific protocols running on top of that network is just as critical. Identifying unencrypted communications, weak credentials, and over-privileged file shares forms the absolute foundation of network vulnerability assessment and exploitation.

---

**Task Status: ✔ Completed**