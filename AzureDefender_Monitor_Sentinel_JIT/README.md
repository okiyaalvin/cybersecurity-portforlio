# ☁️ Azure Security Monitoring & Response – Azure Monitor, Defender for Cloud, JIT VM Access & Microsoft Sentinel

This project demonstrates a complete Azure-native security pipeline—centralized telemetry collection, hardening of virtual machines, time-bound access control, and SIEM-driven detection and automation—implemented across **Labs 08–11**. I configured **Azure Monitor (AMA + Log Analytics + DCR)**, enabled **Microsoft Defender for Servers (Plan 2)**, enforced **Just-In-Time (JIT) VM access**, and onboarded **Microsoft Sentinel** to create analytics rules and playbooks that automatically respond to risky changes (e.g., deletion of a JIT policy). <!-- fileciteturn0file0 -->

![intro](/cybersecurity-portforlio/assets/AzureDefender_Monitor_Sentinel_JIT/intro.png)

---

## 📌 Objective

- Centralize security and performance telemetry from Azure VMs using Azure Monitor (AMA + Log Analytics). <!-- fileciteturn0file0 -->
- Strengthen VM security posture with Microsoft Defender for Cloud (Servers Plan 2). <!-- fileciteturn0file0 -->
- Minimize brute-force exposure by enforcing Just-In-Time, IP-restricted access to VM management ports. <!-- fileciteturn0file0 -->
- Onboard Microsoft Sentinel, connect key data sources, and automate incident response via playbooks. <!-- fileciteturn0file0 -->

---

## 🛠️ Lab Environment

| Role        | System / Service                            | Notes                                                                 |
|-------------|---------------------------------------------|-----------------------------------------------------------------------|
| 🧱 Target    | Azure VM (`myVM`)                           | Deployed in **EastUS**, `AZ500LAB131415` resource group               |
| 🧪 Engineer  | Azure Portal + Cloud Shell (PowerShell)     | Used to deploy resources and run initial provisioning commands        |
| 📊 Telemetry | Log Analytics Workspace + Storage Account   | Centralized log/metric collection; DCR routes performance counters    |
| 🛡️ Protect  | Microsoft Defender for Cloud (Servers Plan 2)| Advanced server protections and recommendations                       |
| 🔐 Access    | Just-In-Time VM Access                      | Time-bound RDP/SSH access with IP scoping                             |
| 🔎 Detect    | Microsoft Sentinel                          | Data connectors, analytics rules, incidents, playbooks                |

**Scope:** Subscription-level configuration in **EastUS** with resource group `AZ500LAB131415`. <!-- fileciteturn0file0 -->

---

## 🧨 Security Focus Summary

- **Visibility:** AMA installed and governed by **Data Collection Rules (DCRs)** forwarding performance/security telemetry to a **Log Analytics Workspace**; diagnostics also persisted to a **Storage Account**. <!-- fileciteturn0file0 -->
- **Hardening:** **Defender for Servers Plan 2** enabled to surface alerts, recommendations, and vulnerability management for Azure/hybrid servers. <!-- fileciteturn0file0 -->
- **Exposure Reduction:** **JIT VM access** enforces short-lived, IP-scoped access to RDP (3389) / SSH (22) with default 3-hour windows. <!-- fileciteturn0file0 -->
- **Detection & Response:** **Sentinel** onboarded with **Azure Activity** connector; a **KQL analytics rule** and **Logic Apps playbook** automatically respond when a **JIT policy is deleted**, escalating incident severity. <!-- fileciteturn0file0 -->

---

## 🧭 Implementation Walkthrough

### 🔎 Lab 08 — Azure Monitor: Log Analytics, Storage Account & DCR
**Goal:** Centralize VM telemetry with AMA + DCR → Log Analytics; persist diagnostics to Storage. <!-- fileciteturn0file0 -->

1) **Provision base resources (Cloud Shell, PowerShell)**
```powershell
# Resource group
New-AzResourceGroup -Name AZ500LAB131415 -Location 'EastUS'

# Opt-in feature (Encryption at host)
Register-AzProviderFeature -FeatureName "EncryptionAtHost" -ProviderNamespace Microsoft.Compute

# VM deployment (opens ports 80, 3389 for lab)
New-AzVm -ResourceGroupName "AZ500LAB131415" -Name "myVM" -Location 'EastUS' `
  -VirtualNetworkName "myVnet" -SubnetName "mySubnet" -SecurityGroupName "myNetworkSecurityGroup" `
  -PublicIpAddressName "myPublicIpAddress" -PublicIpSku Standard -OpenPorts 80,3389 -Size Standard_D2s_v3

# Verify provisioning
Get-AzVM -Name 'myVM' -ResourceGroupName 'AZ500LAB131415' | Format-Table
```
2) **Create Log Analytics Workspace** → *Portal* › **Log Analytics workspaces** › *Create* › Review + Create. <!-- fileciteturn0file0 -->

3) **Create Storage Account** for diagnostics → *Portal* › **Storage accounts** › *Create* › Review + Create. <!-- fileciteturn0file0 -->

4) **Define Data Collection Rule (DCR)**  
*Portal* › **Monitor** › *Data collection rules* › *Create*.  
- **Data source:** *Performance Counters* (defaults)  
- **Destination:** *Azure Monitor Logs* → select the workspace created above  
- Scope the **resource** to `myVM` and *Create*.  
Result: VM telemetry flows to Log Analytics; diagnostics retained in Storage. <!-- fileciteturn0file0 -->

---

### 🛡️ Lab 09 — Microsoft Defender for Cloud (Servers Plan 2)
**Goal:** Enable advanced server protections and insights. <!-- fileciteturn0file0 -->

- *Portal* › **Microsoft Defender for Cloud** › **Environment settings** › select subscription.  
- **Defender plans** › **Cloud Workload Protection (CWP)** › **Servers** → **On** → *Save*.  
> Turning CWP **Servers** to **On** enables **Microsoft Defender for Servers Plan 2** for the subscription. <!-- fileciteturn0file0 -->

Outcome: Enhanced threat detection, security recommendations, and vulnerability management for VMs/servers. <!-- fileciteturn0file0 -->

---

### 🔐 Lab 10 — Enable Just-In-Time (JIT) VM Access
**Goal:** Reduce attack surface by time-bound access to management ports. <!-- fileciteturn0file0 -->

1) **Enable JIT on the VM**
- *Portal* › **Virtual machines** › `myVM` › **Configuration** › **Just-in-time VM access** → **Enable**.  
  - If using a free subscription, you may encounter permissions/plan issues; switching to *Pay-As-You-Go* or using *cloud slices* allows completion. <!-- fileciteturn0file0 -->

2) **Default JIT policy (for reference)**
- **Windows:** RDP **3389**, max access **3 hours**, source IPs **Any**  
- **Linux:** SSH **22**, max access **3 hours**, source IPs **Any** <!-- fileciteturn0file0 -->

3) **Request access to a JIT-enabled VM**
- *Portal* › **Virtual machines** › `myVM` › **Connect** → **Request access** (select IP/time/ports as configured).  
Result: Access is temporarily granted; after expiry, ports are closed automatically. <!-- fileciteturn0file0 -->

---

### 🛰️ Lab 11 — Microsoft Sentinel: Data, Analytics & Automation
**Goal:** Onboard Sentinel, connect data, create a KQL rule + playbook, then trigger/validate an incident. <!-- fileciteturn0file0 -->

1) **Onboard Sentinel**
- *Portal* › **Microsoft Sentinel** › **Create** › select the **Log Analytics Workspace** from Lab 08.  
  > Sentinel requires eligible workspaces; e.g., some workspaces created by Defender for Cloud are not supported. <!-- fileciteturn0file0 -->

2) **Connect the Azure Activity data connector**
- *Sentinel* › **Content hub** → install **Azure Activity** solution, then  
- *Sentinel* › **Data connectors** › **Azure Activity** › **Open connector page** › **Launch Azure Policy Assignment Wizard** to start ingestion (status becomes *Connected* after data arrives). <!-- fileciteturn0file0 -->

3) **Create an analytics rule (scheduled query)**
- *Sentinel* › **Analytics** › **+ Create** › *Scheduled query rule*.  
- **KQL query** to detect deletion of JIT policies:
```kusto
AzureActivity
| where ResourceProviderValue =~ "Microsoft.Security"
| where OperationNameValue =~ "Microsoft.Security/locations/jitNetworkAccessPolicies/delete"
```
- **Query scheduling:** run every **5 minutes** (defaults otherwise). <!-- fileciteturn0file0 -->

4) **Deploy a Logic Apps playbook & automate**
- Deploy the `changeincidentseverity.json` template (*Custom deployment* › **Build your own template in the editor** › **Load file** › deploy).  
- In **Analytics** rule’s **Automated response**, add an **Automation rule** to run the **Change-Incident-Severity** playbook **When alert is created**; grant playbook permissions to the resource group. <!-- fileciteturn0file0 -->

5) **Trigger an incident & validate**
- *Defender for Cloud* › **Workload protections** › **Just-in-time VM access** → for `myVM`, **Remove** JIT.  
- Confirm **Activity log** shows *Delete JIT Network Access Policies*; **Sentinel** **Incidents** should reflect **Medium/High** severity with the playbook executed. <!-- fileciteturn0file0 -->

---

## ⚠️ Impact

- **Visibility & Forensics:** Unified logs/metrics enable faster investigation and baselining. <!-- fileciteturn0file0 -->
- **Risk Reduction:** JIT drastically narrows exposure windows for remote management ports. <!-- fileciteturn0file0 -->
- **Detection & Automation:** Sentinel turns JIT policy tampering into actionable incidents with automated severity changes and response workflows. <!-- fileciteturn0file0 -->

---

## 🔒 Mitigation & Best Practices

- Enforce **least privilege** and **RBAC** for Sentinel, Defender, and VM operations.
- Lock down **NSGs**; deny inbound management traffic except via **JIT**.
- Use **Azure Policy** to require AMA, DCR, and Defender plans across subscriptions.
- Apply **data retention** and **cost governance** on Log Analytics; route noisy sources to Storage.
- Version and review **Logic App** playbooks; require approvals for sensitive automation.

---

## 🖼️ POC

![Azure Monitor – DCR to Workspace](/cybersecurity-portforlio/assets/AzureDefender_Monitor_Sentinel_JIT/dcr-workspace.png)
![Sentinel – Incident After JIT Deletion](/cybersecurity-portforlio/assets/AzureDefender_Monitor_Sentinel_JIT/incident.png)

---

## 📄 Full Report

📥[Download the full report](/cybersecurity-portforlio/AzureDefender_Monitor_Sentinel_JIT/AzureDefender_Monitor_Sentinel_JIT.pdf)

This PDF contains all portal screenshots, commands, wizard settings, KQL, and verification steps.

---

> _"Strong cloud security is equal parts visibility, control, and automation—measure all three."_  
> — Alvin Okiya

[![✔ BACK](https://img.shields.io/badge/BACK_TO_PORTFOLIO-Click_Here-orange?logo=github)](/README.md)
