# 💼 Introductory Networking — TryHackMe (Summary)

This room introduces the fundamental concepts of computer networking, covering theoretical models like OSI and TCP/IP, the process of data encapsulation, and essential command-line networking tools. It provides the foundational knowledge required for understanding how data is transmitted, routed, and secured across networks.

---

## 🌐 Theoretical Networking Models

Understanding how data moves between computers is standardized through two main models:

- **The OSI Model (7 Layers):** A theoretical framework (Application, Presentation, Session, Transport, Network, Data Link, Physical) that breaks down network communication from user interfaces down to electrical hardware signals.
- **The TCP/IP Model (4 Layers):** The practical, older model (Application, Transport, Internet, Network Interface) that serves as the actual basis for real-world networking, condensing the OSI layers into a more streamlined architecture.

---

## 📦 Encapsulation & Protocols

Data must be properly packaged and governed by rules to travel across a network successfully.

- **Encapsulation / De-encapsulation:** As data moves down the layers for transmission, it receives specific headers and trailers (transforming from data to segments/datagrams, packets, frames, and bits). The receiving computer then reverses this process.
- **TCP (Transmission Control Protocol):** Connection-based and highly reliable. Uses a "three-way handshake" (SYN, SYN/ACK, ACK) to establish a stable connection and ensure data accuracy without loss.
- **UDP (User Datagram Protocol):** Connectionless and fast. Sends packets at the target without verifying receipt, making it ideal for speed-dependent tasks like video streaming.

---

## 🛠️ Essential Networking Tools

Command-line utilities are vital for troubleshooting, mapping, and investigating networks.

- **Ping:** Uses the ICMP protocol on the Network/Internet layer to test if a connection to a remote resource is alive and responding.
- **Traceroute:** Maps the exact path and intermediate servers (hops) a request takes to reach its final destination.
- **WHOIS:** Queries domain registrars to extract registration details and organizational information about a specific domain name.
- **Dig:** Interrogates DNS (Domain Name System) servers—moving from local caches to Recursive and Authoritative servers—to manually resolve URLs into IP addresses and troubleshoot records.

---

## 🧠 Career Direction Insight

This room reinforces the core principles necessary for bridging physical infrastructure management with cybersecurity research. 

- **Current Learning Focus:** Network fundamentals, OSI/TCP/IP models, packet encapsulation, and command-line troubleshooting tools.
- **Application:** A strong grasp of these layers directly supports advanced engineering tasks, such as configuring OSPF and RIP protocols or effectively troubleshooting inter-VLAN routing on setups like a single router connected to a VLAN 90 switch.

---

## 📊 Key Takeaways

- The OSI model provides a 7-layer theoretical guide, while TCP/IP is the practical 4-layer architecture running the internet.
- Encapsulation standardizes data transmission, adding necessary routing information and security verification at each layer.
- Protocol selection matters: TCP prioritizes reliable, verified data delivery, whereas UDP prioritizes high-speed transmission.
- Tools like Ping, Traceroute, WHOIS, and Dig form the foundation of diagnosing network infrastructure and performing early-stage reconnaissance.

---

## 🧠 Final Summary

Mastering networking basics is indispensable for any computer engineering or security discipline. From analyzing packet structures at the Data Link layer to mapping out DNS records, this foundational theory enables better network defense, more efficient routing configurations, and a deeper understanding of how modern digital infrastructure operates.

---

**Task Status: ✔ Completed**