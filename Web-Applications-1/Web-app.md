# 🌐 Web Applications – OWASP Top 10 & Vulnerability Discovery Lab

This lab isn’t just about ticking off security boxes — it’s about understanding the DNA of modern web application threats. Designed to simulate live vulnerabilities using platforms like DVWA, HTB Academy, or custom-built lab setups, this project places you in the shoes of both attacker and defender.

From broken authentication to SQL injection, you explored **OWASP Top 10 vulnerabilities**, their exploit chains, and the underlying misconfigurations that made them possible. It’s the kind of knowledge that transforms a cybersecurity enthusiast into a truly observant analyst.

---

## 📌 Objective

- Explore the most critical vulnerabilities affecting real-world web applications
- Learn how improper input validation, broken access controls, and misconfigured sessions lead to compromise
- Execute structured manual attacks to discover and understand security flaws
- Propose technical remediation based on secure coding principles

---

## 🧪 Lab Components

- 🌐 Target Platforms:
  - DVWA (Damn Vulnerable Web Application)
  - HTB Academy Web Module
  - TryHackMe environments (where applicable)
  - Custom web server deployments

- 🔍 Tools Used:
  - Burp Suite, OWASP ZAP, Firefox Dev Tools  
  - curl, sqlmap, cURL  
  - Browser extensions for cookie manipulation

- 🔐 Focused Attacks:
  - Cross-Site Scripting (XSS) – Stored and reflected  
  - SQL Injection – Boolean-based, error-based, and union exploitation  
  - Command Injection – Remote OS command execution  
  - Broken Authentication – Weak password enforcement, session hijacking  
  - Security Misconfiguration – Open directories, verbose errors, exposed config files  
  - File Upload Bypass – MIME-type spoofing and content validation flaws

---

## 🚦 Exploitation Highlights

### 🔹 SQL Injection

Used `' OR 1=1--` payload in login forms to bypass authentication. Leveraged `sqlmap` to automate data extraction from vulnerable endpoints and reviewed query responses in Burp Suite.

### 🔹 XSS Exploits

Injected `<script>alert('xss')</script>` into comment fields, search boxes, and feedback forms. Observed alert boxes in DOM and tested for cookie theft via document.cookie.

### 🔹 Command Injection

Abused unsanitized ping forms to run:
```
127.0.0.1 && whoami
```
Captured output confirming backend execution privileges. Explored remote access capabilities with crafted payloads.

### 🔹 File Upload Flaw

Uploaded `.php.jpg` payload with embedded shell code. Server validation failed to block executable content. Shell deployed and triggered via browser query.

---

## 🛠️ Remediation Techniques

- Input Sanitization using regex and whitelisting  
- Server-side filtering vs client-side validation  
- Content Security Policy (CSP) headers for XSS  
- Secure session tokens with rotation and expiry  
- Disable verbose error reporting in production environments  
- File upload restrictions by MIME type, content inspection, and sandboxing

---

## 🧠 Key Takeaways

- Web applications are intricate systems, often patched together with third-party code, legacy logic, and misconfigured frameworks  
- Vulnerabilities stem from **assumptions in trust and input**  
- Manual testing is irreplaceable — tools can assist, but human logic finds the hidden flaws  
- Secure design isn’t just one fix — it’s a mindset woven into **every form, route, response, and config file**

This lab pushed you deeper into the logic layer, peeling back how applications actually behave under pressure — and what signs attackers look for in flawed systems.

---

## 🖼️ Screenshots

![image](/assets/web-applications-1/info-disc.jpg)  

---
![XSS Alert Trigger](/assets/web-applications-1/xss-alert.png)  

---
![File Upload Exploit Shell](/assets/web-applications-1/exploit-js.jpg)

---

## 📄 Full Report

📥 [Download the full Web Applications lab documentation (PDF)](/Switch%20&%20Router%20Network%20Configuration/Switch%20&%20Router%20Network.pdf)

Includes request/response logs, payload breakdowns, mitigation proposals, and screenshots for each vulnerable component explored.

---

> _"Secure code isn't the absence of bugs — it's the presence of discipline, design, and doubt."_  
> — Alvin Okiya

[![✔ BACK](https://img.shields.io/badge/BACK_TO_PORTFORLIO-Click_Here-darkgreen?logo=github)](/README.md)