# 💼 OWASP API Security Top 10-1 — TryHackMe

This room introduces the first half of the OWASP API Security Top 10, highlighting the unique vulnerabilities that arise when APIs act as the direct conduit between client applications and backend data. 

---

## 🔑 Authorization & Authentication Flaws
Many API breaches occur not through complex exploitation, but because the API implicitly trusts the client's requests.

*   **Broken Object Level Authorization (BOLA):** The API fails to verify if the user requesting an object (like a specific employee ID) actually has permission to view it. Attackers can simply increment ID numbers in the URL to access other users' data.
*   **Broken User Authentication (BUA):** Mishandling authentication, such as sending credentials or tokens in `GET` request URLs rather than secure headers, allowing attackers to intercept or brute-force access tokens.
*   **Broken Function Level Authorization (BFLA):** The API relies on the client to hide administrative functions. Attackers can manipulate hidden fields (like `isAdmin`) or directly guess admin endpoints to execute privileged actions.

---

## 🗄️ Data Exposure & Resource Management
APIs frequently over-deliver data or fail to protect backend resources from exhaustion.

*   **Excessive Data Exposure:** The backend sends entire data objects (including sensitive info like internal device IDs or hidden usernames) to the client, relying on the frontend application to filter what the user actually sees. This must be fixed programmatically at the API level, not just via network devices.
*   **Lack of Resources & Rate Limiting:** Failing to limit the number or frequency of requests an API can process. This opens the door to brute-force attacks (like spamming an OTP endpoint) and Denial of Service (DoS). While firewalls can mitigate this, rate limiting should be enforced natively by the API.

---

## 🧠 Engineering Insight
Securing APIs requires a hybrid approach. While infrastructure-level controls (like the ACLs and firewalls in a multi-area OSPF network) can help mitigate rate-limiting abuse, they cannot inspect the context of the data. When designing backend databases like SQL Server, the API must act as a strict gatekeeper. If an API suffers from BOLA or Excessive Data Exposure, it will blindly query and return every record from your meticulously structured database, completely bypassing your intended application logic.

---

**Task Status: ✔ Completed**