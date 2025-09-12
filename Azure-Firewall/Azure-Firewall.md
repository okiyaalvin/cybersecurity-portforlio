# 🔥 Azure Firewall Deployment & Traffic Control – Full Lab Walkthrough

![image](/assets/Azure-Firewall/Azure-firewall.png)

This lab is a comprehensive deep-dive into deploying and configuring **Azure Firewall** as a central security control in a cloud-based virtual network. It simulates a real-world scenario where outbound traffic must be tightly regulated using **custom routing**, **application rules**, and **network rules** — all orchestrated through Azure’s native services.

The lab was executed in the **East US region** using a custom ARM template, and it showcases how to build a secure perimeter around cloud workloads while maintaining visibility and control over traffic flows.

---

## 🎯 Lab Objective

- Deploy a virtual network with multiple subnets and virtual machines  
- Provision Azure Firewall and route all outbound traffic through it  
- Configure application rules to allow access only to approved domains  
- Set up network rules to enable DNS resolution via external servers  
- Test firewall enforcement using Remote Desktop and browser access  
- Clean up resources post-deployment to avoid unnecessary charges

---

## 🧱 Environment Overview

| Component        | Details                          |
|------------------|----------------------------------|
| Region           | East US                          |
| Resource Group   | `AZ500LAB08`                     |
| Virtual Network  | `Test-FW-VN`                     |
| Subnets          | `Workload-SN`, `Jump-SN`, `AzureFirewallSubnet` |
| VMs              | `Srv-Work`, `Srv-Jump`           |
| Firewall         | `Test-FW01`                      |
| Public IP        | `TEST-FW-PIP`                    |
| Route Table      | `Firewall-route`                 |

---

## 🛠️ Step-by-Step Execution

### 🧩 Task 1: Deploy Lab Environment via ARM Template

- Navigated to Azure Portal → searched for **"Deploy a custom template"**
- Loaded JSON template from GitHub:  
  [AZ500 Lab Template](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/blob/master/Allfiles/Labs/08/template.json)
- Uploaded template → filled parameters → deployed to `AZ500LAB08`

✅ Result: Virtual network, subnets, and VMs successfully provisioned

---

### 🔥 Task 2: Deploy Azure Firewall

- Created `Test-FW01` firewall in `Test-FW-VN`  
- Selected **Standard SKU** with zone redundancy  
- Assigned public IP: `TEST-FW-PIP`  
- Disabled management NIC for simplicity

✅ Result: Firewall deployed with private IP `10.0.1.4`

---

### 🛣️ Task 3: Create Default Route

- Created route table: `Firewall-route`  
- Added route:
  - Name: `FW-DG`
  - Address Prefix: `0.0.0.0/0`
  - Next Hop Type: `Virtual Appliance`
  - Next Hop IP: `10.0.1.4` (Firewall’s private IP)
- Associated route table with `Workload-SN` subnet

✅ Result: All outbound traffic from workload subnet now flows through firewall

---

### 🌐 Task 4: Configure Application Rule

- Created rule collection: `App-Coll01`  
- Source: `10.0.2.0/24`  
- Protocols: `http:80`, `https:443`  
- Target FQDNs: `www.bing.com`  
- Action: `Allow`, Priority: `200`

✅ Result: Only traffic to Bing is allowed; other domains are blocked

---

### 🧭 Task 5: Configure Network Rule

- Created rule collection: `Net-Coll01`  
- Source: `10.0.2.0/24`  
- Destination: `209.244.0.3`, `209.244.0.4`  
- Protocol: `UDP`, Port: `53`  
- Action: `Allow`, Priority: `200`

✅ Result: DNS resolution allowed only through specified external servers

---

### 🧬 Task 6: Configure VM DNS Servers

- Navigated to `Srv-Work` → Networking → Network Interface  
- Set DNS servers to:
  - Primary: `209.244.0.3`
  - Secondary: `209.244.0.4`

✅ Result: VM now uses external DNS servers defined in firewall rules

---

### 🧪 Task 7: Test Firewall Enforcement

- Connected to `Srv-Jump` via RDP → then to `Srv-Work`  
- Disabled IE Enhanced Security Configuration  
- Accessed:
  - ✅ `www.bing.com` → successful
  - ❌ `www.microsoft.com` → blocked (no matching rule)

✅ Result: Firewall correctly enforced application and network rules

---

### 🧹 Task 8: Clean Up Resources

- Opened Azure Cloud Shell → selected PowerShell  
- Ran:
  ```powershell
  Remove-AzResourceGroup -Name "AZ500LAB08" -Force -AsJob
  ```

✅ Result: All lab resources deleted to prevent billing charges

---

## 🧠 Key Takeaways

- Azure Firewall provides **stateful L3–L7 filtering** with centralized policy control  
- Application rules allow domain-based filtering — ideal for web access control  
- Network rules enable protocol-specific traffic control (e.g., DNS, RDP)  
- Custom routing ensures traffic enforcement without relying on default system routes  
- DNS configuration on VMs must align with firewall rules to avoid resolution failures  
- Testing via RDP and browser access validates real-world enforcement

This lab demonstrates how to build a **secure, scalable, and auditable cloud perimeter** using native Azure tools — a must-have skill for cloud security engineers and SOC analysts.

---

## 📸 Screenshots

![Firewall Deployment Overview](/assets/Azure-Firewall/deployment-overview.png)  
![Application Rule Configuration](/assets/Azure-Firewall/app-rule.png)  
![Network Rule for DNS](/assets/Azure-Firewall/dns-rule.png)  
![Blocked Access to Microsoft](/assets/Azure-Firewall/blocked-ms.png)

---

## 📄 Full Report

📥 [Download the full Azure Firewall lab documentation (PDF)](/Azure-Firewall/Azure-firewall.pdf)

Includes deployment steps, screenshots, rule configurations, and validation results.

---

> _"A firewall isn’t just a gate — it’s a policy engine, a visibility lens, and a guardian of trust."_  
> — Alvin Okiya

---

[![🔙 Back to Main Portfolio README](https://img.shields.io/badge/←_Return_to_Portfolio-Click_Here-maroon?logo=github)](/README.md)