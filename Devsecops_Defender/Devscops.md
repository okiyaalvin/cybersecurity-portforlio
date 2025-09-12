# 🐚 DevSecOps Defender – Azure Sentinel Integration Lab

![image](/assets/Devsecops_Defender/devsecops-int.png)

This project demonstrates the integration of DevSecOps principles into a cloud-native security operations workflow using Azure Sentinel. It showcases how automated detection, response, and governance can be embedded into CI/CD pipelines to enhance visibility, reduce response time, and enforce compliance across hybrid environments.

---

## 📌 Objective
- Implement DevSecOps workflows using Azure Sentinel and Microsoft Defender
- Automate threat detection and response in CI/CD pipelines
- Simulate attack scenarios and validate detection rules
- Integrate security telemetry from multiple sources into Sentinel
- Document and visualize the security lifecycle from code to cloud

---

## 🛠️ Lab Environment

| Role        | System / App                  | Notes                                      |
|-------------|-------------------------------|--------------------------------------------|
| 🧱 Target    | Azure DevOps, AKS, GitHub      | CI/CD pipeline with containerized workloads |
| 🧪 Attacker  | Kali Linux, Burp Suite, Nmap   | Simulated attacker probing exposed services |

---

## 🧨 Vulnerability Summary

This lab focuses on misconfigured CI/CD pipelines and exposed container workloads that lack proper runtime security controls. The vulnerabilities stem from:

- Unrestricted access to build artifacts
- Lack of image scanning and policy enforcement
- Inadequate logging and alerting for container activity
- Over-permissive service principals and role assignments

---

## 🧭 Exploitation Walkthrough

![image](/assets/Devsecops_Defender/azure-devsecops.png)

### 1. Reconnaissance & Enumeration
Simulated attacker scans exposed AKS services:
```bash
    nmap -sV -p 80,443 <target-ip>
```
Identifies open ports and misconfigured ingress controller.
### 2. Exploiting Misconfigured CI/CD Pipeline
Attacker accesses unsecured build artifacts via GitHub Actions:
```bash
    curl https://github.com/<repo>/actions/artifacts/<id>
```
Extracts secrets embedded in environment variables.
### 3. Privilege Escalation via Service Principal
Uses leaked credentials to authenticate:
```bash
    az login --service-principal -u <client-id> -p <secret> --tenant <tenant-id>
```
Enumerates resources and modifies Sentinel rules.

### 4. Sentinel Alert Trigger
Azure Sentinel detects anomalous login and triggers playbook: Sentinel Alert Dashboard

### 5. Automated Response
Playbook disables compromised service principal and sends notification
```json
{
  "action": "disable",
  "target": "servicePrincipal",
  "status": "success"
}
```
##  ⚠️ Impact
- Unauthorized access to build artifacts and secrets
- Privilege escalation across Azure resources
- Tampering with Sentinel detection rules
- Potential lateral movement into production workloads
- Loss of integrity in CI/CD pipeline

##  🔒 Mitigation Recommendations
- Enforce least privilege on service principals and roles
- Integrate image scanning and policy checks in CI/CD
- Enable Sentinel alerting for identity-based anomalies
- Use secure storage for secrets and environment variables
- Audit and rotate credentials regularly
- Implement conditional access and MFA for DevOps accounts

## POC
![image](/assets/Devsecops_Defender/Azure-devsecops-res.png)  
![image](/assets/Devsecops_Defender/git-scan.png)  
![image](/assets/Devsecops_Defender/git-yml.png)  
![image](/assets/Devsecops_Defender/bandit.png)

---

📄 Full Report  
📥[Download the full report](/Devsecops_Defender/Devsecops_Defender.pdf)

---

>_"Security isn’t a gate — it’s a thread woven through every commit."_  
>— Alvin Okiya


[![✔ BACK](https://img.shields.io/badge/BACK_TO_PORTFORLIO-Click_Here-blue?logo=github)](../README.md)
