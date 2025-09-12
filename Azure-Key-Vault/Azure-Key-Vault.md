# 🔐 Azure Key Vault + Always Encrypted – End-to-End Data Protection Lab

![image](/assets/Azure-Key-Vault/azure-key-vault.png)

This lab demonstrates how to build a **secure data solution in Microsoft Azure** by integrating **Azure Key Vault** with **Azure SQL Database Always Encrypted**. It simulates a real-world scenario where sensitive data (like SSNs and birthdates) must be encrypted both at rest and in transit — while ensuring that encryption keys are centrally managed and never exposed to the database engine.

From infrastructure deployment (Iac using ARM template) to application-level encryption, this lab showcases how to combine **cloud-native security services**, **identity management**, and **developer tooling** to achieve true data confidentiality.

---

## 🎯 Lab Objectives

- Deploy infrastructure using an ARM template (VM + SQL DB)
- Create and configure Azure Key Vault with keys and secrets
- Register an application in Microsoft Entra ID for secure access
- Encrypt SQL database columns using Always Encrypted with Key Vault integration
- Build and run a .NET console app that securely accesses encrypted data
- Validate encryption and query encrypted columns using SSMS and the app

---

## 🧱 Environment Overview

| Component              | Details                            |
|------------------------|-------------------------------------|
| Region                 | East US                            |
| Resource Group         | `AZ500LAB10`                       |
| VM                    | `az500-10-vm1` (Windows Server 2022) |
| SQL Server             | `sqlservermas6nsrnkokgo`           |
| SQL Database           | `medical`                          |
| Key Vault              | `az500kv1090236040`                |
| App Registration       | `sqlApp`                           |
| Entra ID User          | `okiyaalvin_gmail.com#EXT#@...`    |

---

## 🚀 Phase 1: Infrastructure Deployment

- Used Azure Portal → “Deploy a custom template”  
- Loaded `az-500-10_azuredeploy.json` from GitHub  
- Created VM with Visual Studio + SSMS pre-installed  
- Provisioned SQL Server and `medical` database  
- Captured credentials:  
  - Username: `Student`  
  - Password: `C0mpl3xP@ssw0rd!`

✅ Result: All resources deployed successfully in `AZ500LAB10`

---

## 🔐 Phase 2: Key Vault Setup

### 🔹 Create Key Vault

```powershell
$kvName = 'az500kv' + $(Get-Random)
$location = (Get-AzResourceGroup -ResourceGroupName 'AZ500LAB10').Location
New-AzKeyVault -VaultName $kvName -ResourceGroupName 'AZ500LAB10' -Location $location -DisableRbacAuthorization
```

✅ Vault URI: `https://az500kv1090236040.vault.azure.net/`

### 🔹 Configure Access Policies

- Granted full Key, Secret, and Certificate permissions to:
  - `Alvin Okiya` (personal account)
  - `student@okiyaalvingmail.onmicrosoft.com` (Entra ID user)
  - `sqlApp` (registered application)

---

## 🔑 Phase 3: Key & Secret Creation

### 🔹 Add RSA Key

```powershell
$key = Add-AZKeyVaultKey -VaultName $kv.VaultName -Name 'MyLabKey' -Destination 'Software'
```

- Key Type: RSA  
- Size: 2048 bits  
- Identifier: `https://az500kv1090236040.vault.azure.net/keys/MyLabKey/...`

### 🔹 Add SQL Password Secret

```powershell
$secretvalue = ConvertTo-SecureString 'Pa55w.rd1234' -AsPlainText -Force
Set-AZKeyVaultSecret -VaultName $kv.VaultName -Name 'SQLPassword' -SecretValue $secretvalue
```

✅ Secret stored securely in Key Vault

---

## 🧠 Phase 4: App Registration & Permissions

- Registered `sqlApp` in Microsoft Entra ID  
- Captured:
  - Application (Client) ID: `aae5eeb6-115b-48ac-96f1-01049411d3b1`
  - Secret (Key1): `KDT8Q~QN6ULrmkEibyYotZRq-OvqvRC...`

### 🔹 Grant App Access to Key Vault

```powershell
Set-AZKeyVaultAccessPolicy -VaultName $kvName -ResourceGroupName AZ500LAB10 -ServicePrincipalName $applicationId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list
```

✅ App now authorized to use Key Vault keys

---

## 🧬 Phase 5: SQL Database Encryption

### 🔹 Create Patients Table

```sql
CREATE TABLE [dbo].[Patients](
  [PatientId] INT IDENTITY(1,1),
  [SSN] CHAR(11) NOT NULL,
  [FirstName] NVARCHAR(50),
  [LastName] NVARCHAR(50),
  [BirthDate] DATE NOT NULL,
  PRIMARY KEY CLUSTERED ([PatientId] ASC)
);
```

### 🔹 Encrypt Columns via SSMS Wizard

- Selected `SSN` → Deterministic  
- Selected `BirthDate` → Randomized  
- Chose Azure Key Vault as key store  
- Signed in with Entra ID user  
- Created:
  - Column Master Key: `CMK_Auto1`
  - Column Encryption Key: `CEK_Auto1`

✅ Encryption completed successfully

---

## 💻 Phase 6: Console App Integration

### 🔹 Built .NET Console App in Visual Studio

- Installed NuGet packages:
  - `Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider`
  - `Microsoft.IdentityModel.Clients.ActiveDirectory`

- Used connection string with Always Encrypted enabled  
- Inserted sample patient data into encrypted columns  
- Queried encrypted SSN column securely

### 🔹 Sample Output

```
Signed in as: aae5eeb6-115b-48ac-96f1-01049411d3b1
Enter server password: ********
Adding sample patient data...
Patient found with SSN = 999-99-0003
Donna Carreras | Birthdate: 2/9/1973
```

✅ Data encrypted in SQL, decrypted only by authorized app

---

## 🧹 Phase 7: Resource Cleanup

```powershell
Remove-AzResourceGroup -Name "AZ500LAB10" -Force -AsJob
```

✅ All resources deleted to prevent billing charges

---

## 🧠 Key Takeaways

- Azure Key Vault enables centralized, secure key management  
- Always Encrypted ensures data confidentiality even from DBAs  
- Entra ID integration enforces identity-based access control  
- Console apps can securely query encrypted columns using Key Vault  
- Real-world encryption requires orchestration across identity, infrastructure, and application layers

This lab is a blueprint for **zero-trust data protection** in the cloud — combining encryption, identity, and access control into one seamless pipeline.

---

## 📸 Screenshots

![Key Vault Overview](/assets/Azure-Key-Vault/Master-key-vault.png)  
![Encrypted Columns Wizard](/assets/Azure-Key-Vault/encrypt-columns.png)  
![Console App Output](/assets/Azure-Key-Vault/console-app-output.png)  
![SSMS Query Results](/assets/Azure-Key-Vault/SSMS-Encryption.png)

---

## 📄 Full Report

📥 [Download the full Azure Key Vault + Always Encrypted lab documentation (PDF)](/Azure-Key-Vault/azure-key-vault.pdf)

Includes deployment logs, screenshots, PowerShell commands, SQL scripts, and application code walkthrough.

---

> _"Encryption is not just a checkbox — it’s a commitment to protecting what matters, even from those who manage it."_  
> — Alvin Okiya

---

[![🔙 Back to Main Portfolio README](https://img.shields.io/badge/←_Return_to_Portfolio-Click_Here-orange?logo=github)](/README.md)