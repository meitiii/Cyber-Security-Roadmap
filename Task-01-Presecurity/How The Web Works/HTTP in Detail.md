# 🌐 Room: HTTP in Detail (TryHackMe)

This room focused on understanding how the HTTP/HTTPS protocols work, how clients communicate with web servers, and how requests and responses are structured during web communication.

HTTP (**HyperText Transfer Protocol**) is the protocol used for transferring web content between clients and servers. HTTPS is the secure version of HTTP, where communication is encrypted using SSL/TLS to protect data from interception and tampering.

The room also explained the structure of a URL and its important components:
- **Scheme** → Defines the protocol (`http`, `https`, `ftp`)
- **Host** → Domain name or IP address
- **Port** → Communication port (`80` for HTTP, `443` for HTTPS)
- **Path** → Resource location on the server
- **Query String** → Additional parameters sent to the application
- **Fragment** → Reference to a specific section inside a webpage

Several important HTTP methods were introduced:
- **GET** → Retrieve data from a server
- **POST** → Submit data or create new resources
- **PUT** → Update existing information
- **DELETE** → Remove resources from the server
- **TRACE** → Debugging and request tracing

The room covered HTTP status codes and their meanings:
- **200 OK** → Request completed successfully
- **201 Created** → New resource created
- **301/302 Redirect** → Resource moved temporarily or permanently
- **400 Bad Request** → Invalid client request
- **401 Unauthorized** → Authentication required
- **403 Forbidden** → Access denied
- **404 Not Found** → Requested page does not exist
- **500 Internal Server Error** → Server-side failure
- **503 Service Unavailable** → Server overloaded or under maintenance

Another major topic was HTTP headers, which provide additional information between the client and server.

### Common Request Headers
- `Host` → Specifies the target website
- `User-Agent` → Identifies the browser/client
- `Content-Length` → Size of transmitted data
- `Accept-Encoding` → Supported compression methods
- `Cookie` → Sends stored session information

### Common Response Headers
- `Set-Cookie` → Stores cookies on the client
- `Content-Type` → Defines returned data type
- `Cache-Control` → Controls browser caching
- `Content-Encoding` → Compression method used

The room also explained how cookies work. Since HTTP is stateless, cookies help web applications remember users, maintain sessions, and handle authentication by storing unique identifiers on the client side.

In the practical section, different HTTP requests such as GET, POST, PUT, and DELETE were manually performed to interact with web resources, modify user data, authenticate users, and retrieve challenge flags.

## 🧠 Key Takeaways
- HTTP is the core protocol used for web communication.
- HTTPS secures communication using encryption.
- URLs contain multiple components such as scheme, host, port, and query strings.
- HTTP methods define the action performed on a resource.
- Status codes indicate the result of a request.
- Headers provide additional metadata between client and server.
- Cookies are commonly used for authentication and session management.
- Manual HTTP requests are essential for web testing and security analysis.

---

**Task Status:** ✅ Completed