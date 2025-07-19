# 🛡️ Azure NSG & ASG Lab – Role-Based Network Access Control

![image](/assets/NSG-ASG/NSG-ASG-1.png)

This lab simulates a real-world scenario where an organization needs to **segregate access between Web Servers and Management Servers** in Azure using **Network Security Groups (NSGs)** and **Application Security Groups (ASGs)**. The goal was to enforce **role-specific access policies**, allowing RDP only to management servers and HTTP/HTTPS access only to web servers — all while maintaining centralized control and visibility.

The lab was executed in the **East US region**, and every step — from virtual network creation to rule enforcement and validation — was performed manually through the Azure Portal.

---

## 🎯 Lab Objectives

- Create a secure virtual network infrastructure using NSGs and ASGs  
- Segment servers into logical groups for policy enforcement  
- Allow RDP access only to management servers  
- Allow HTTP/HTTPS access only to web servers  
- Validate access control through real-world testing  
- Clean up resources to prevent billing charges

---

## 🧱 Infrastructure Overview

| Component               | Details                          |
|------------------------|----------------------------------|
| Resource Group          | `AZ500LAB07`                     |
| Virtual Network         | `myVirtualNetwork`               |
| Subnet                 | `default (10.0.0.0/24)`          |
| NSG                    | `myNsg`                          |
| ASG – Web Servers       | `myAsgWebServers`                |
| ASG – Management Servers| `myAsgMgmtServers`               |
| VMs                    | `myVmWeb`, `myVMMgmt`            |

---

## 🛠️ Step-by-Step Execution

### 🔹 Task 1: Create Virtual Network

- Created `myVirtualNetwork` with address space `10.0.0.0/16`  
- Subnet: `default` with `10.0.0.0/24`  
- Deployed in `East US` region  
- Verified deployment via Azure Portal

---

### 🔹 Task 2: Create Application Security Groups

- Created `myAsgWebServers` for web-facing VMs  
- Created `myAsgMgmtServers` for internal management VMs  
- Both ASGs deployed in same region and resource group  
- Purpose: Logical grouping for NSG rule targeting

---

### 🔹 Task 3: Create Network Security Group

- Created NSG named `myNsg`  
- Associated NSG with `default` subnet of `myVirtualNetwork`  
- NSG acts as the central policy engine for inbound traffic

---

### 🔹 Task 4: Configure Inbound NSG Rules

#### ✅ Rule 1: Allow HTTP/HTTPS to Web Servers

| Setting              | Value               |
|----------------------|---------------------|
| Destination          | `myAsgWebServers`   |
| Port                 | `80,443`            |
| Protocol             | `TCP`               |
| Priority             | `100`               |
| Name                 | `Allow-Web-All`     |

#### ✅ Rule 2: Allow RDP to Management Servers

| Setting              | Value               |
|----------------------|---------------------|
| Destination          | `myAsgMgmtServers`  |
| Port                 | `3389`              |
| Protocol             | `TCP`               |
| Priority             | `110`               |
| Name                 | `Allow-RDP-All`     |

---

### 🔹 Task 5: Deploy Virtual Machines

#### 🖥️ Web Server VM – `myVmWeb`

- OS: Windows Server 2022  
- Size: Standard D2s v3  
- Public IP: Enabled  
- NSG: None (handled via subnet-level NSG)  
- Disk: Standard HDD  
- Installed IIS via RunPowerShellScript:
  ```powershell
  Install-WindowsFeature -name Web-Server -IncludeManagementTools
  ```

#### 🖥️ Management Server VM – `myVMMgmt`

- OS: Windows Server 2022  
- Size: Standard D2s v3  
- Public IP: Enabled  
- NSG: None (handled via subnet-level NSG)  
- Disk: Standard HDD

---

### 🔹 Task 6: Associate VMs to ASGs

- Associated `myVmWeb` NIC to `myAsgWebServers`  
- Associated `myVMMgmt` NIC to `myAsgMgmtServers`  
- Verified ASG membership via VM → Networking → Application Security Groups

---

## 🧪 Access Validation

### ✅ RDP to Management Server

- Used Azure Portal → Connect → RDP  
- Downloaded `.rdp` file and authenticated using:
  - Username: `Student`  
  - Password: (created during VM setup)  
- Encountered initial error due to missing NSG association  
- Resolved by re-associating NSG to subnet  
- Successfully connected after fix

### ✅ HTTP/HTTPS to Web Server

- Identified public IP of `myVmWeb`  
- Accessed via browser:
  ```
  http://<public-ip>
  ```
- IIS welcome page displayed — confirming successful web access

---

## 🧹 Resource Cleanup

To avoid billing charges, ran the following in Azure Cloud Shell (PowerShell):

```powershell
Remove-AzResourceGroup -Name "AZ500LAB07" -Force -AsJob
```

✅ Resources deleted in background job

---

## 🧠 Key Takeaways

- **ASGs** simplify rule targeting by grouping VMs logically  
- **NSGs** enforce traffic rules at subnet or NIC level  
- Role-based access control is achievable with layered security groups  
- Testing access paths (RDP, HTTP) is essential for validation  
- Troubleshooting requires checking NSG associations and rule priorities  
- Cleaning up resources is a best practice for cost control

This lab reflects real-world cloud security engineering — where precision, visibility, and validation define success.

---

## 📸 Screenshots

![NSG Rule for Web Servers](/assets/NSG-ASG/web-NSG.png)  

![NSG Rule for RDP Access](/assets/NSG-ASG/RDP-NSG.png) 

![IIS Page Access](/assets/NSG-ASG/iis.png)  

![RDP Connection Success](/assets/NSG-ASG/rdp-success.png)

---

## 📄 Full Report

📥 [Download the full NSG & ASG lab documentation (PDF)](/NSG-ASG/NSG-ASG.pdf)

Includes portal walkthroughs, rule configurations, VM deployment steps, troubleshooting logs, and validation screenshots.

---

> _"Security isn’t just about blocking — it’s about allowing the right access to the right roles, at the right time."_  
> — Alvin Okiya

---

[![🔙 Back to Main Portfolio README](https://img.shields.io/badge/←_Return_to_Portfolio-Click_here-blue?logo=github)](/README.md)