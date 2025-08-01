# 🧩 CIS Benchmark Assessment – Microsoft 365 Configuration (IG1)

This case study documents a CIS Controls Self Assessment (CSAT) conducted for a mid-sized cloud-first organization. The assessment focused on **Implementation Group 1 (IG1)** controls across Microsoft 365 and supporting infrastructure, with the goal of establishing a secure operational baseline for identity, access, data, and endpoint protection.

---

## 🛠️ Scope of Assessment

- 🌐 **Microsoft 365 Tenant**  
  - Azure AD, Exchange Online, SharePoint Online, Microsoft Teams  
  - Defender for Office 365  

- 💻 **Endpoint Environment**  
  - **Windows OS** for general users  
  - **Kali Linux** for technical and developer roles  

- 📋 **CIS Controls Evaluated**  
  - Controls C01 to C17 from the CIS v8 Benchmark  
  - Focused exclusively on **Implementation Group 1 (IG1)** safeguards  

---

## 🔍 Methodology

- Manual review of CIS IG1 controls using the CIS CSAT tool  
- Validation of control status across four dimensions:  
  - **Policy Defined**  
  - **Control Implemented**  
  - **Control Automated**  
  - **Control Reported**  
- Evidence-based mapping of safeguards to organizational practices  
- Anonymized findings for external sharing  

---

## 🧩 IG1 Control Highlights

| Control Area                     | Summary of Findings                                                                 |
|----------------------------------|--------------------------------------------------------------------------------------|
| **Asset Management (C01)**       | Asset inventory partially implemented; unauthorized asset handling not enforced     |
| **Software Inventory (C02)**     | Inventory exists; support tracking and unauthorized software handling need work     |
| **Data Protection (C03)**        | Strong access controls; encryption on end-user devices not enforced                 |
| **Secure Configuration (C04)**   | Session locking, firewalls, and secure management mostly in place                   |
| **Account Management (C05)**     | Unique passwords and account inventories enforced; dormant account handling in progress |
| **Access Control (C06)**         | MFA enforced across remote, admin, and external access; access provisioning not automated |
| **Vulnerability Management (C07)**| Patch management partially automated; remediation strategy not fully defined        |
| **Audit Logging (C08)**          | Logs collected and stored adequately; audit process documentation informal          |
| **Browser & Email Security (C09)**| Supported clients used; DNS filtering not implemented                               |
| **Malware Defenses (C10)**       | Anti-malware deployed; signature updates and removable media controls partially automated |
| **Data Recovery (C11)**          | Backup processes in place; isolated recovery instances not fully implemented        |
| **Network Infrastructure (C12)** | Infrastructure kept up-to-date; automation in place for most systems                |
| **Security Awareness (C14)**     | Training on authentication and data handling exists; phishing and insecure network training lacking |
| **Service Provider Management (C15)** | Inventory exists; automation and policy coverage limited                        |
| **Incident Response (C17)**      | Roles and contacts defined; reporting process partially implemented                 |

---

## 📊 Control Maturity Summary

| Maturity Dimension     | % Coverage Across IG1 Controls |
|------------------------|-------------------------------|
| **Policy Defined**     | ~70% (many informal or partial) |
| **Control Implemented**| ~80% (most systems covered)     |
| **Control Automated**  | ~50% (automation varies widely) |
| **Control Reported**   | ~75% (reporting mostly consistent) |

---

## 📎 Remediation Highlights

- Enforced MFA across all externally exposed and administrative interfaces  
- Improved firewall and session locking configurations across Windows and Kali Linux endpoints  
- Strengthened data access controls and inventory tracking  
- Initiated policy development for unauthorized asset and software handling  
- Enhanced patch management automation for OS and applications  
- Created internal documentation for audit logging and incident response  

---

## 🧠 Analyst Reflection

> _"This IG1-focused assessment revealed strong foundational security practices, but also highlighted areas where automation and policy formalization are needed. The findings will guide future remediation and compliance efforts."_  
> — Cybersecurity Analyst

---

## 📁 Evidence Documentation
All findings were validated through internal documentation, system configurations, and audit logs. Evidence is retained privately and not included in this public summary.

[![🧩 Back to Portforlio](https://img.shields.io/badge/CIS_Assessment-Back_To_Portfolio-green?logo=microsoft)](/README.md)


```markdown