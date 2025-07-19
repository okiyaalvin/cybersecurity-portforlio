# 🛡️ AWS Privilege Escalation by Rollback – CloudGoat Exploitation

![image](/assets/AWS-privesc/aws-rollback.png)

This isn’t just another lab — it’s a surgical deep-dive into the cracks and blind spots of cloud security. In this project, I escalated AWS IAM privileges by rolling back to a legacy policy version using **CloudGoat’s iam_privesc_by_rollback** scenario. Crafted inside a Kali Linux VM with AWS CLI and Terraform, the setup emulated real-world exploitation steps: misconfigurations, policy drift, overlooked rollback rights — all culminating in **root-level access from a restricted account**.

This exercise was intense, unforgiving, and revealing — a true analyst’s trial by fire.

---

## 📌 Mission Objective

- Simulate an **IAM privilege escalation via policy rollback** inside AWS  
- Identify exploitable configurations in IAM user permissions  
- Revert to an older, highly privileged policy using `set-default-policy-version`  
- Transition from limited access to full admin without alerting controls  
- Document every step as evidence of risk exposure

---

## 🧪 Environment & Tooling

- 🔐 **CloudGoat** vulnerable scenarios (iam_privesc_by_rollback)  
- 💻 Kali Linux VM w/ AWS CLI and Terraform installed  
- 🌩️ AWS environment (profile name: `cgoat`)  
- 🔍 Tools used: `aws-cli`, manual policy enumeration, shell scripting  
- 👤 Target IAM user: `raynor`  
- 🎯 Vulnerable policy: `cg-raynor-policy-cgidrk@ius9uzo`  
- 🧠 Exploitation strategy: Switch default policy from least privilege to **Action: "*" Resource: "*"**

---

## 🧩 Strategy Breakdown

### 🔹 Initial State: Raynor's Restricted Access

- Could only **view** IAM resources and **switch policy versions**
- Denied EC2 and sensitive AWS actions
- Verified with:
  ```
  aws ec2 describe-instances --profile raynor
  ```

🚫 Result: UnauthorizedOperation error — textbook PoLP in effect

---

### 🔹 Enumeration Phase: Policy Version Recon

IAM policies in AWS support up to 5 versions. Only one is active. Older versions may retain **forgotten privileges**. Here’s what I discovered:

| Version | Permissions Summary                                |
|---------|----------------------------------------------------|
| v1      | List IAM + Switch Policy Version (🚩 rollback allowed) |
| v2      | Deny all unless from specific IPs (restrictive)    |
| v3      | ✅ Full Admin: `Action: "*"` on `Resource: "*"`       |
| v4      | Date-restricted read-only                          |
| v5      | S3 Read-only access                                |

🔍 Bingo: **v1 lets rollback**, **v3 grants root** — the perfect storm.

---

### 🔓 Exploitation: The Rollback Jump

**Privilege Escalation Achieved With:**

```bash
aws iam set-default-policy-version \
--policy-arn arn:aws:iam::493544649029:policy/cg-raynor-policy-cgidrk@ius9uzo \
--version-id v3 \
--profile raynor
```

✅ Output confirmed switch to v3  
✅ Re-ran EC2 command — succeeded instantly  
✅ Visibility into full AWS ecosystem unlocked

---

### 🧪 Post-Escalation Actions

- 🛠️ Created new IAM user: `pentest-user`  
- 🎯 Attached `AdministratorAccess` directly  
- 🧠 Confirmed persistent privilege — now Raynor was root, permanently

Commands used:

```bash
aws iam create-user --user-name pentest-user --profile raynor
aws iam attach-user-policy --user-name raynor-cgidrk@ius9uzo \
--policy-arn arn:aws:iam::aws:policy/AdministratorAccess \
--profile raynor
```

This wasn’t just access escalation. It was **identity takeover and trust chain overwrite**.

---

## 📉 Risk Analysis

- Policy versioning is often ignored — older versions linger like dormant bombs  
- IAM permissions that allow `SetDefaultPolicyVersion` can grant **backdoor escalation**  
- Rollbacks bypass usual approval flows — especially in environments without CloudTrail alerts  
- Once access is gained, attackers can **mint credentials**, **create resources**, or **wipe logs**

---

## 🛡️ Mitigation Recommendations

| Control                    | Purpose                                  |
|---------------------------|------------------------------------------|
| 🔒 Remove old policy versions | Prevent rollback to insecure rules       |
| 📜 Tighten IAM permissions   | Avoid giving version switching capability |
| 🧠 Audit IAM via CloudTrail  | Detect unexpected role or version changes |
| 🚨 Enforce MFA & alerts      | Catch suspicious actions in real-time     |
| 📆 Schedule policy reviews   | Eliminate stale access grants            |

---

## 📸 Evidence
- Policy Versions

![Policy Versions Listed](/assets/AWS-privesc/policy-versions.png)  

---
- Restricted access before the escalation of priviledges

![Before Escalation – Access Denied](/assets/AWS-privesc/b4escalation.jpg)  

---
- After escalation we could now view content on ec2

![After Escalation – EC2 Visible](/assets/AWS-privesc/after-escalation.jpg)  

---
- Also tested by creating a user called pentest-user

![Creating Pentest User](/assets/AWS-privesc/pentestuser.png)

---

## 📄 Full Report

📥 [Download the full AWS IAM Rollback Exploitation report (PDF)](/AWS-Priviledge-Escalation/W7A1-c-r.pdf)

Includes full CLI logs, exploit commands, screenshots, and mitigation strategies.

---

> _"Cloud permissions don’t scream when they’re misused — they whisper. And if you’re listening, you’ll hear the breach before it explodes."_  
> — Alvin Okiya

[![🔙 Back to Main Portfolio README](https://img.shields.io/badge/←-Return_to_Portfolio-orange?logo=github)](/README.md)

