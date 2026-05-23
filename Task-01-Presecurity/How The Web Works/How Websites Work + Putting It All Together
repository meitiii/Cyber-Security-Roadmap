# 🌐 Room: How Websites Work + Putting It All Together (TryHackMe)

This room explains how websites function from a full-stack perspective, including how browsers interact with servers, how web technologies work together, and how different infrastructure components support modern web applications.

When a user visits a website, the browser sends a request to a web server using the HTTP protocol. The server responds with data such as HTML, CSS, JavaScript, and media files, which the browser then renders into a complete webpage. This process involves multiple layers of technologies working together seamlessly.

Websites are built using three core frontend technologies:
- **HTML** → Defines the structure of the webpage
- **CSS** → Handles styling and visual design
- **JavaScript** → Adds interactivity and dynamic behavior

The internet also relies on a client-server architecture where:
- The **Frontend (Client-side)** runs in the browser and displays the website.
- The **Backend (Server-side)** processes requests, handles logic, and returns responses.

---

## 🌍 How Web Pages Are Structured

HTML (HyperText Markup Language) forms the foundation of all websites. It uses elements (tags) such as:
- `<html>` → Root of the document
- `<head>` → Metadata and page information
- `<body>` → Visible content
- `<h1>`, `<p>`, `<img>`, `<button>` → Content elements

HTML elements can also include attributes like:
- `class` → For styling multiple elements
- `id` → Unique identifier for a single element
- `src` → Specifies image or file source

JavaScript enhances HTML by adding interactivity. It can modify page content dynamically, respond to user actions (like clicks), and update the DOM in real time.

---

## ⚠️ Security Concepts

### Sensitive Data Exposure
Sensitive data exposure occurs when developers accidentally leave important information (like credentials or hidden links) in frontend code. Since HTML and JavaScript are visible to users, attackers can inspect source code to find hidden data.

### HTML Injection
HTML injection happens when user input is not properly sanitized and is directly rendered on a webpage. Attackers can inject HTML or JavaScript code to manipulate the page or display malicious content.

---

## 🔁 How a Web Request Works (Full Flow)

When a user requests a website:

1. DNS resolves the domain into an IP address.
2. The browser sends an HTTP/HTTPS request to the server.
3. The server processes the request and responds with data.
4. The browser renders the response into a webpage.

---

## ⚙️ Web Infrastructure Components

### Load Balancers
Load balancers distribute incoming traffic across multiple servers to ensure reliability and performance.
- Use algorithms like round-robin or weighted distribution
- Perform health checks to ensure servers are online

### CDN (Content Delivery Network)
A CDN stores static files (images, CSS, JS) across global servers, delivering content from the nearest location to improve speed and reduce latency.

### Databases
Databases store and manage website data. Common examples include MySQL, MongoDB, PostgreSQL, and others.

### WAF (Web Application Firewall)
A WAF protects websites by filtering malicious traffic, blocking attacks, and applying rate limiting to prevent abuse.

---

## 🖥️ Web Server Concepts

A web server is software that serves web content using HTTP. Common examples include Apache, Nginx, IIS, and NodeJS.

### Virtual Hosts
Virtual hosts allow one server to host multiple websites by mapping domain names to different directories.

### Static vs Dynamic Content
- **Static Content** → Fixed files (HTML, CSS, images)
- **Dynamic Content** → Generated on demand using backend logic

Dynamic content is processed by backend languages such as PHP, Python, Ruby, and NodeJS, which interact with databases and external services.

---

## 🧠 Key Takeaways

- Websites rely on client-server architecture.
- DNS, HTTP, and web servers work together to deliver web pages.
- HTML, CSS, and JavaScript form the frontend of websites.
- Backend systems handle logic, data processing, and dynamic content.
- Security issues like sensitive data exposure and HTML injection arise from improper input handling.
- Infrastructure components like load balancers, CDNs, and WAFs improve performance and security.
- Virtual hosts allow multiple websites on a single server.
- Modern web applications combine static and dynamic content for flexibility and interactivity.

---

**Task Status:** ✅ Completed