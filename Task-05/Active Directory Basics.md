# 🏢 Active Directory Basics — Runbook

## 📝 Executive Summary
This room covers the architectural foundations of Windows Active Directory (AD). It explains how AD centralizes identity management, policy enforcement, and resource access, as well as how authentication protocols (Kerberos vs. NetNTLM) and multi-domain structures (Trees/Forests/Trusts) function in enterprise environments.

---

## 🌐 Core Concepts
* **Domain Controller (DC):** The server that runs AD services and hosts the directory database.
* **Objects:** The building blocks of AD, including Users, Machines, Groups, and Printers.
* **Security Principals:** Objects (Users, Computers, Groups) that can be authenticated and assigned privileges.
* **Organizational Units (OU):** Containers used to group objects for hierarchical management and the application of Group Policy Objects (GPOs).
* **Trust Relationships:** Logical links between domains that allow authentication/authorization across boundaries (Trees and Forests).

---

## 🛠️ Practical Reference

### AD Management & Delegation
* **Machine Account Naming:** `COMPUTERNAME$`
* **Delegation:** Granting specific administrative privileges (like password resets) to low-privileged users for specific OUs without making them Domain Admins.
* **Accidental Deletion:** Protects OUs; must be disabled in `View > Advanced Features > Object Properties` to delete.

### Authentication Protocols
* **Kerberos (Default/Modern):** Uses a Key Distribution Center (KDC) to issue Tickets (TGT/TGS) to access services.
* **NetNTLM (Legacy):** Uses a challenge-response mechanism. Does *not* transmit the password or hash over the network.

### Key Groups
* **Domain Admins:** Full administrative control over the entire domain.
* **Enterprise Admins:** Control over all domains within an entire forest.

---

## 🛡️ Remediation & Best Practices
* **Least Privilege:** Use delegation to assign specific tasks (e.g., password resets) to IT staff rather than granting full Domain Admin rights.
* **OU Structure:** Mimic business units (Sales, IT, Management) for efficient GPO deployment.
* **GPO Distribution:** Use the `SYSVOL` network share to propagate policies to all domain machines.
* **Authentication Security:** Transition away from legacy NetNTLM where possible to reduce exposure to relay attacks.

---

## 🧠 Key Takeaways
* **OUs vs. Groups:** OUs are for **policy application** (GPOs) and hierarchy; Groups are for **access control** (permissions over files/printers).
* **Trust Direction:** In a one-way trust, if Domain A trusts Domain B, users from B can access resources in A. The trust direction is opposite to the access direction.
* **Kerberos Flow:** TGT (Ticket Granting Ticket) allows the user to request a TGS (Ticket Granting Service) for specific resources, minimizing the need to handle sensitive credentials repeatedly.

---
**Task Status:** ✅ Completed