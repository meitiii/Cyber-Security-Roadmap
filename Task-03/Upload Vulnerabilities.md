# 📤 Upload Vulnerabilities — TryHackMe

This room explores the practical exploitation of file upload forms, covering techniques to bypass various client-side and server-side filters to achieve Remote Code Execution (RCE).

---

## 🛠️ Bypass Methodologies
Security professionals must understand how to chain enumeration with specific bypass techniques to gain unauthorized access.

*   **Client-Side Bypass:** JavaScript filters running in the browser can be defeated by disabling JS, intercepting/modifying the request in Burp Suite (e.g., changing `Content-Type` to `text/x-php`), or uploading directly to the server endpoint.
*   **Extension Filtering (Server-Side):** If a server blocks `.php`, attackers often test for alternative extensions (e.g., `.php5`, `.phtml`) that the web server might still execute.
*   **Magic Numbers (File Signatures):** When a filter validates the file type based on its "Magic Number" (the first few bytes of the file), attackers can use `hexeditor` to prepend the bytes of a valid image (e.g., GIF/JPEG) to a malicious PHP script.

---

## 🚀 Exploitation Workflow
To turn an uploaded file into a reverse shell, follow these systematic steps:

1.  **Reconnaissance:** Use `gobuster` to map the target and identify the directory where uploaded files are stored (e.g., `/resources`, `/images`, `/privacy`).
2.  **Payload Preparation:** Create a PHP or Node.js reverse shell script (e.g., `shell.php`). Ensure the IP/port match your local `tun0` interface and listener.
3.  **Bypass & Upload:** Use the appropriate bypass technique (Magic Number, Extension manipulation, or Burp Intercept) to ensure the server accepts the malicious file.
4.  **Listener Setup:** Start a local listener (e.g., `nc -lvnp 443`) to receive the connection.
5.  **Execution:** Navigate to the uploaded file's URL (e.g., `http://target.thm/images/shell.php`) to trigger the execution and spawn the reverse shell.

---

## 🧠 Engineering Insight
File upload vulnerabilities are a masterclass in why **input validation should never be performed solely on the client side**. Even with server-side checks, developers must be wary of "blacklisting" extensions or MIME types, as attackers will always find an unblocked extension (like `.php5`) or a way to spoof content types. The most robust defense is to store uploaded files outside the web root, rename them to randomly generated strings, and use strict allow-listing for file types after verifying the file content itself.

---

## 📊 Key Takeaways
*   **Enumeration is Critical:** Without `gobuster` to find the exact directory where the web server saves uploads, the shell file remains unreachable.
*   **Intercepting Requests:** Burp Suite is indispensable for viewing and altering the `Content-Type` header, which is often the primary gatekeeper for simple uploads.
*   **Persistence:** Once RCE is achieved, the shell allows full access to the file system (e.g., browsing `/var/www` to retrieve flags), reinforcing the criticality of restricting file execution permissions on upload directories.

---

**Task Status: ✔ Completed**