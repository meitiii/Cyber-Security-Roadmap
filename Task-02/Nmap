# 💼 Nmap: The Basics — TryHackMe (Summary)

This room introduces the fundamentals of Nmap, the industry-standard open-source network scanner. It covers how to discover live hosts on a network, perform various port scanning techniques, detect operating systems and service versions, and efficiently manage scan timing and output formats for reporting.

---

## 🌐 Host Discovery & Scanning Techniques

Nmap allows you to target specific IPs, ranges (e.g., `192.168.0.1-10`), subnets (`192.168.0.1/24`), or hostnames. 

- **Host Discovery (`-sn`):** Also known as a ping scan. Uses ARP for local networks and ICMP/TCP for remote networks to find active devices without port scanning.
- **TCP Connect Scan (`-sT`):** Completes the full TCP 3-way handshake. Reliable but noisy, as target logs will record the connection. It is the default scan if run without root privileges.
- **SYN Stealth Scan (`-sS`):** Sends a SYN packet and resets the connection before completion. It is faster and stealthier (less likely to be logged) but requires root (`sudo`) privileges.
- **UDP Scan (`-sU`):** Checks for open UDP ports (like DNS). Slower and more difficult since UDP is connectionless and doesn't require a handshake.

---

## 🛠️ Enumeration, Timing & Output Controls

Once a host is discovered, Nmap can extract deep technical details and save the data efficiently.

- **Port Selection:** Scans 1,000 top ports by default. Use `-F` for the top 100, `-p 22,80` for specific ports, or `-p-` for all 65,535 ports.
- **Advanced Detection:** 
  - `-O`: Guesses the target's Operating System.
  - `-sV`: Detects the exact software and version running on open ports.
  - `-A`: Aggressive scan (combines OS detection, versioning, script scanning, and traceroute).
  - `-Pn`: Treats the host as online, forcing a scan even if it blocks ping requests.
- **Timing (`-T0` to `-T5`):** Controls scan speed. `-T0` is extremely slow/paranoid to avoid detection, while `-T4` (Aggressive) is commonly used for fast, reliable networks.
- **Output Formats:** Save results using `-oN` (Normal), `-oG` (Grepable for bash tools), `-oX` (XML), or `-oA` to output all major formats simultaneously. Use `-v` for verbosity and `-d` for debugging.

---

## 🧠 Career Direction Insight

Mastering Nmap bridges the gap between infrastructure management and offensive security reconnaissance.

- **Current Learning Focus:** Transitioning from theoretical networking (OSI/TCP) to practical network mapping, service enumeration, and vulnerability discovery.
- **Application:** For someone configuring complex routing (OSPF/RIP) and inter-VLAN environments, Nmap is the ultimate auditing tool. It verifies whether ACLs, firewall rules, and VLAN segmentations are actually blocking unintended traffic or exposing vulnerable services.

---

## 📊 Key Takeaways

- Nmap behaves differently based on privileges; running with `sudo` unlocks stealthy and raw-packet features like the `-sS` (SYN) scan.
- Relying solely on `ping` is inefficient for discovery, as modern firewalls drop ICMP requests; `-Pn` bypasses this limitation.
- The `grepable` (`-oG`) and `XML` (`-oX`) output formats are essential for automating security workflows and parsing results into other penetration testing tools.
- Granular control over timing (`-T`) and port selection (`-p-`) allows for balancing stealth, speed, and accuracy during an engagement.

---

## 🧠 Final Summary

Nmap is the foundational tool for network reconnaissance. Whether you are a system administrator verifying local network security or a penetration tester mapping out an external attack surface, understanding Nmap's flags for discovery, enumeration, and output generation is a mandatory skill for any cybersecurity professional.

---

**Task Status: ✔ Completed**