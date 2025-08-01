# 🛡️ VAPT Engagement Showcase – Alvin Okiya

## 📌 Overview
Summary of Vulnerability Assesments & Penetration Testing (VAPT) engagements conducted across multiple industries. All data is anonymized to protect client confidentiality.

---

## 🧪 Case Study 001 – Industrial Manufacturing (Internal Network)

**Client Profile**: Mid-sized enterprise in industrial gases  
**Scope**: Internal network across HQ and 3 branches  
**Tools Used**: Nmap, Nessus, manual CVE analysis  
**Standards**: PTES, OWASP, PCI DSS

### 🔍 Key Findings
- 8 Critical: Obsolete SQL Server, HP iLO CVEs, SMB RCE, SNMP cleartext
- 10 Severe: SMB signing disabled, deprecated SMBv1, self-signed certs
- 1 Moderate: SSH CBC cipher

### 🛠️ Remediation
- Upgraded legacy services
- Enforced SMB signing
- Replaced self-signed certs
- Hardened SNMP and SSH

### 🧠 Reflections
> “This engagement taught me the importance of legacy system visibility and chaining infrastructure-level vulnerabilities for lateral movement.”

---

## 🧪 Case Study 002 – Retail & Consumer Electronics (Web + DNS)

**Scope**: Public-facing website + DNS  
**Tools Used**: OWASP ZAP, DNS scripts  
**Findings**:  
- 2 Medium: Anti-CSRF tokens missing, CSP header absent  
- 6 Low: Cookie flags, error disclosure  
- DNS: No DNSSEC, DMARC set to quarantine

### 🛠️ Remediation
- Implemented secure headers
- Upgraded TLS to v1.3
- Recommended DNSSEC and DMARC hardening

---

## 🧪 Case Study 003 – E-commerce Platform (Merchant + Customer Portals)

**Scope**: Two web apps + DNS infrastructure  
**Tools Used**: OWASP ZAP, dig, dnsenum, spiderfoot  
**Findings**:  
- 5 Medium: Anti-CSRF, CSP, hidden files  
- DNS: Misconfigured HINFO, no SRV, subdomain exposure

### 🛠️ Remediation
- Hardened headers and cookies
- Recommended DNSSEC and mail server hardening

---

## 🧪 Case Study 004 – Fintech Platform (Dev Phase)

**Scope**: SCF platform during development  
**Findings**:  
- 7 Medium: CSP misconfigurations, missing headers  
- 6 Low: Insecure cookies, server info leakage

### 🛠️ Remediation
- Provided roadmap for header and cookie security
- Aligned platform with OWASP standards

---

## 🧪 Case Study 005 – Fintech Platform (Live App)

**Scope**: Web app hosted at supplychain.kcbgroup.com  
**Tools Used**: Burp Suite, SQLmap, Nikto  
**Findings**:  
- 4 High: Default admin credentials, force browsing  
- 20 Medium: Outdated JS libraries, missing headers

### 🛠️ Remediation
- Upgraded libraries
- Secured authentication flows
- Implemented rate limiting and proper headers

---

## 🧪 Case Study 006 – Educational Retail (Web + DNS)

**Scope**: Public-facing site + DNS  
**Tools Used**: OWASP ZAP, dig, spiderfoot  
**Findings**:  
- 2 Medium: Anti-CSRF, CSP  
- 7 Low: Cookie flags, timestamp leaks  
- DNS: SPF misconfigured, subdomains exposed

### 🛠️ Remediation
- Hardened headers and cookies
- Recommended DNSSEC, DKIM, DMARC
- Advised Cloudflare proxying for subdomains

---

## 🧠 Analyst Reflections

- Developed reusable reporting templates
- Improved DNS enumeration workflows
- Learned to communicate technical risks to non-technical teams
- Strengthened understanding of OWASP and PTES methodologies

---

## 📎 Disclaimer

> All case studies are anonymized and generalized to protect client confidentiality. They are intended to demonstrate methodology, tooling, and my perspective as an analyst — not to disclose specific vulnerabilities or systems.


[![🔙 Back to Main Portfolio README](https://img.shields.io/badge/←-Return_to_Portfolio-orange?logo=github)](/README.md)