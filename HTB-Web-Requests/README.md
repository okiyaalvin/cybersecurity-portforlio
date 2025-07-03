# 🌐 HTB Academy – Web Requests

This lab covers foundational concepts of HTTP and HTTPS, focusing on their role in web application communication. Through the **HTB Academy Web Requests module**, I explored request methods, response structures, headers, cURL usage, browser devtools, and simulated real-world interactions to reveal how critical it is to understand HTTP from a **security assessment** perspective.

---

## 📌 Objective

- Grasp the structure and usage of **HTTP vs HTTPS**
- Understand how to craft and inspect **web requests and responses**
- Learn to manipulate requests using **cURL**, browser dev tools, and CLI utilities
- Simulate **basic authentication**, **response header extraction**, and **API-based CRUD operations**

---

## 🛠️ Tools & Resources

- 🧪 HTB Academy Web Requests Module  
- 🧰 `curl`, `jq`, browser dev tools (Network tab)  
- 🌐 Test endpoints and mock APIs  
- 🗂️ JSON manipulation techniques, HTTP status codes

---

## 🔍 Lab Activities & Concepts

### 1. **Anatomy of an HTTP URL**
Covered components like:
- Scheme: `http://`, `https://`
- User Info: e.g., `admin:password@`
- Host & Port: `inlanefreight.com:80`
- Path, Query String, Fragment

### 2. **Using `curl` to Inspect Requests**
- `curl -O`: download files  
- `curl -v`: view full request/response  
- `curl -I`: send `HEAD` request for headers only  
- `curl -k`: bypass SSL verification on self-signed certs

### 3. **HTTP Methods**
| Method | Purpose |
|--------|---------|
| `GET`    | Retrieve resources |
| `POST`   | Send data to server |
| `PUT`    | Update existing resource |
| `DELETE` | Remove resource |
| `HEAD`   | Fetch headers only |
| `OPTIONS`, `PATCH` | Discover or modify behavior dynamically |

### 4. **Request & Response Dissection**
- Learned to interpret the HTTP request line, headers, and body  
- Used dev tools to capture `POST /login.php` and extract forms and cookies  
- Reviewed 2xx (success), 3xx (redirect), 4xx (client error), 5xx (server error)

### 5. **Authentication Testing**
- Basic HTTP Auth with:
  ```bash
  curl -u admin:admin http://host:port/login
  ```
- Observed `Authorization: Basic <base64>` headers

---

## 🧪 Practical Exercises

- ✅ Downloaded files using GET requests and `curl`  
- ✅ Inspected headers to identify server versions (`Apache/2.4.41`)  
- ✅ Analyzed HTTPS handshake flow and differences from HTTP  
- ✅ Found flag values using browser **Network tab** and crafted exact requests with cURL  
- ✅ Used sessions and cookies with JSON POST:
  ```bash
  curl -X POST http://IP:PORT/search.php \
  -H "Content-Type: application/json" \
  -H "Cookie: PHPSESSID=your-session-cookie" \
  -d '{"search": "flag"}'
  ```

### 🧠 Explored:
- Response headers like `Strict-Transport-Security`, `Content-Security-Policy`, `Referrer-Policy`
- HTTP Status codes like `200 OK`, `302 Found`, `403 Forbidden`, `500 Internal Server Error`
- Common **CRUD API** interactions with POST/GET/PUT/DELETE

---

## 🧠 Key Takeaways

- HTTP is not just for browsers — it’s a gateway for **manual exploitation**, **automation**, and **API analysis**
- `curl` is invaluable for replaying, modifying, and understanding requests at a forensic level
- Authentication, session management, and data leaks are often visible in plaintext under insecure transport (HTTP)
- Knowing the difference between request methods empowers you to:
  - Upload or delete data using `PUT`, `DELETE`
  - Use `HEAD` to validate content presence
  - Understand how content is fetched dynamically via APIs and AJAX calls

---

## 🖼️ Screenshots

_Add image files to this folder and link them like this:_

```markdown
![Header Inspection with curl -I](./curl-header-inspect.png)
![DevTools POST Analysis](./devtools-post-req.png)
```

---

## 📄 Full Report

📥 [Download the full HTB Web Requests lab documentation (PDF)](./HTB-Web-Requests.pdf)

The report includes cURL command logs, screenshots of browser devtools, authentication headers, response captures, and walkthroughs of each request manipulation task.

---

> _"If you're analyzing apps but ignoring HTTP traffic, you're only seeing half the story."_  
> — Alvin Okiya