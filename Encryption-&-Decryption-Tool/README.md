# 🔐 Encryption & Decryption Tool – Final Year University Project

This project marks a major milestone in my academic journey — a capstone I completed in my **fourth year of the BSc in Computer Security and Forensics** at Meru University. It fulfilled the mandatory graduation requirement and represents the culmination of years of learning in cryptography, secure coding, and software development principles.

The result: a functional **GUI-based encryption and decryption tool** built in Python using the Fernet module from the `cryptography` library. It’s designed for **confidentiality-first file protection**, easy enough for everyday users but built on strong security practices.

---

## 📌 Objective

- Build an accessible tool for file encryption and decryption using modern cryptographic libraries
- Apply real-world knowledge of cryptographic algorithms and key management
- Demonstrate both technical proficiency and secure software design in a final-year deliverable

---

## 🛠️ Technologies & Libraries

- 🔤 **Python 3.10+**
- 📦 `cryptography` module (Fernet symmetric encryption)
- 🖼️ `tkinter` for GUI interface
- 📁 Integrated file selection, status messages, and success confirmation

---

## 🎯 Core Features

### ✅ File Encryption Workflow
- Select target file (.txt, .csv, .docx, etc.)
- Generate or input encryption key
- Encrypt file and save output with `.enc` extension

### ✅ File Decryption Workflow
- Select previously encrypted file
- Input valid Fernet key
- Restore original file contents to new file

### ✅ GUI Interface
- Minimal design built with `tkinter`
- Visual buttons for encryption & decryption
- Error and success messages for usability

### 🧠 Crypto Logic Behind the Scenes
- Uses **symmetric encryption** with a **256-bit key**
- Key can be generated and stored separately by the user
- Ensures both **data confidentiality** and **basic integrity**

---

## 🧪 Testing & Validation

- Verified encryption/decryption across multiple file types
- Simulated lost key scenarios and observed failure behavior
- Ensured no residual plaintext remained in temporary files
- Compared original vs decrypted file hashes for data fidelity

---

## 📄 Project Purpose & Reflection

This tool was designed as more than an academic exercise. It demonstrates my understanding of:
- How encryption fits into larger **data protection strategies**
- The value of **user-friendly secure software**
- Real-world applicability of cybersecurity principles I’ve studied since first year

It was submitted as part of my **graduation portfolio** in the final semester of my Computer Security and Forensics degree — one of the most defining projects of my time at Meru University.

---

## 🖼️ Screenshots
- Encrypted and Decrypted file

![Encryption Interface](/assets/Encryption&Decryption-tool/encrypted%20file.png)  

![Decryption Confirmation](/assets/Encryption&Decryption-tool/Decrypted-file.png)

---

## 📄 Full Report

📥 [Download the full Encryption & Decryption Tool documentation (PDF)](/Encryption-&-Decryption-Tool/Encryption&Decryption%20tool.pdf)

Includes design diagrams, source code structure, GUI implementation steps, testing matrices, and submission statement from Meru University.

---

> _"Security isn’t built with just theory — it’s crafted through code, tested with intent, and deployed with purpose."_  
> — Alvin Okiya