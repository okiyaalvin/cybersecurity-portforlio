# 🌐 Network Traffic Analysis with Wireshark

This project is a hands-on exploration of how different network protocols behave at the packet level using Wireshark. The aim was to capture and analyze traffic generated through real-life network activities such as pinging, DNS resolution, and TCP connections. Through this lab, I developed the skill to interpret what "normal" traffic looks like and how this baseline visibility can later support threat detection.

---

## 📌 Objective

The goal of this lab was to:
- Gain proficiency in using **Wireshark** for packet capture and analysis
- Understand the structure and behavior of **ICMP**, **TCP**, and **DNS** traffic
- Build foundational skills in **network forensics** and **protocol dissection**

---

## 🛠️ Tools & Environment

- **Wireshark**: GUI packet analyzer tool used to capture and dissect real-time traffic
- **Kali Linux**: Source machine used to initiate traffic (e.g., pings and DNS queries)
- **Windows endpoint**: Target host for communication
- **Isolated lab network**: Ensured that traffic remained private and controlled

---

## 🔍 Lab Activities & Observations

### 1. ICMP Echo Request/Reply (Ping)
- Sent ICMP Echo Requests from Kali to Windows
- Observed **Type 8** (echo request) and **Type 0** (echo reply) messages
- ICMP traffic had **no ports**, since it operates at the Network Layer

### 2. TCP Three-Way Handshake
- Initiated an HTTP request to a mock web server
- Captured the **SYN → SYN-ACK → ACK** handshake sequence
- Checked for **TCP flags**, port numbers, and sequence numbers

### 3. DNS Query Analysis
- Performed `dig` and browser-based DNS requests to external domains
- Identified **standard queries (0x0000)** and **recursive responses**
- Reviewed TTL, response code (NOERROR), and resolved IPs

### 4. Filtering Techniques in Wireshark
- Used filters like:
  - `icmp`
  - `tcp.port == 80`
  - `dns.qry.name == "example.com"`
- Color coding in Wireshark helped differentiate protocols at a glance

---

## 🧠 Key Takeaways

- **ICMP** is used for diagnostics and reachability (e.g., ping) and can help identify live hosts.
- **TCP** behavior (flags, retransmissions, resets) reveals application state and is useful for traffic baselining.
- **DNS** logs provide valuable insight into user intent and can be monitored for DGA (Domain Generation Algorithm) behavior.
- **Wireshark** is not just a capture tool — it's a visibility engine for network-based threat detection and behavioral baselining.
- Understanding protocol behavior is crucial in detecting anomalies like **suspicious beaconing**, **data exfiltration**, or **malicious C2 communication**

---

## 🖼️ Screenshots

_Add image files to this folder and link them here:_

```markdown
![ICMP Packet Capture](./icmp-capture.png)
![DNS Query Breakdown](./dns-query.png)

---
## 📄 Full Report

📥 [Download the detailed PDF documentation](./Wireshark-Traffic-Analysis.pdf)

The attached document includes timestamped packet traces, annotated screenshots, and insights into each protocol breakdown. Perfect for recruiters or colleagues who want a deep technical dive.

---

> _“The difference between guessing and knowing starts with looking at the packet.”_  
> — Alvin Okiya
