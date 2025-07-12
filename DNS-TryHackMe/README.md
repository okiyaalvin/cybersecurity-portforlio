# 🌐 DNS in Detail – TryHackMe Lab

This project documents my hands-on experience with the TryHackMe lab titled **"DNS in Detail."** It covers foundational DNS concepts, practical record lookups, enumeration techniques, and how DNS can be used in reconnaissance during penetration testing or threat hunting.

---

## 📌 Objective

The goal of this lab was to:
- Deepen my understanding of the **Domain Name System (DNS)** and how it functions
- Explore record types (A, AAAA, NS, CNAME, MX, TXT, SOA)
- Learn techniques for passive and active **DNS reconnaissance**
- Practice using tools like `nslookup`, `dig`, and **online OSINT services**

---

## 🛠️ Tools & Resources

- 🧪 [TryHackMe: DNS in Detail](https://tryhackme.com/room/dnsindetail)
- 🖥️ Kali Linux terminal
- 🔍 CLI tools: `dig`, `host`, `nslookup`, `whois`
- 🧰 Bonus: `dnsenum`, `dnsrecon`, `sublist3r`, `crt.sh`

---

## 🔍 Lab Activities & Commands

### 1. Basic DNS Lookups
- Queried A, MX, and TXT records using `dig` and `nslookup`
- Identified the **mail exchanger** and **SPF records** for specific domains
- Analyzed DNS TTL values and SOA parameters

### 2. Zone Transfers & DNS Reconnaissance
- Attempted zone transfers using:
  ```bash
  dig axfr @target-nameserver domain.com
  ```
- Used `dnsrecon` for brute-forcing subdomains
- Tested for misconfigured name servers and recursive queries

### 3. OSINT Tactics
- Investigated certificate transparency logs using `crt.sh`
- Used `whois` and domain registration info to build a threat actor profile
- Discussed DNS-based attack vectors (e.g., DNS tunneling, DGA, typosquatting)

---

## 🧠 Key Takeaways

- DNS plays a central role in **both attack surface expansion** and **network visibility**
- Public-facing DNS records can unintentionally leak subdomains, internal services, or email configurations
- Attackers can exploit unsecured DNS for tunneling, exfiltration, or C2 callbacks
- Defensive DNS monitoring (e.g., detection of abnormal TXT lookups) is essential for **SOC analysts**

---

## 🖼️ POC


![image](/assets/DNS-TryHackme/DNS.jpg)

---

![image](/assets/DNS-TryHackme/DNS1.jpg)

---

## 📄 Full Report

📥[Download the full DNS in Detail lab documentation (PDF)](/DNS-TryHackMe/DNS-TryHackMe.pdf)

The report includes full command outputs, annotated screenshots, enumeration findings, and reflections on defensive and offensive DNS usage.

---

> _"When you understand DNS, you unlock one of the internet’s most powerful maps — and one of its most overlooked vulnerabilities."_  
> — Alvin Okiya