# 🎯 MAC Flooding & ARP Spoofing – Network Attack Simulation

This lab dives into the aggressive side of network manipulation — simulating **MAC flooding** and **ARP spoofing** attacks to illustrate how fragile Layer 2 and 3 trust mechanisms can be when left unchecked. Built using **Cisco Packet Tracer**, the exercise demonstrates how attackers exploit switches and ARP protocols to **intercept traffic**, **degrade performance**, and **poison routing tables**.

It’s more than just a demo — it’s a wake-up call on how easy it can be to disrupt LAN communication and siphon off sensitive packets if proper defenses aren’t in place.

---

![image](/assets/MacFlooding-&-ARPspoofing/passive-Net-sniff.jpg)

---

## 📌 Objective

- Replicate MAC flooding and ARP spoofing in a controlled lab environment
- Monitor how switches and hosts react to malicious traffic manipulations
- Understand attack vectors and identify signs of compromise
- Implement countermeasures to minimize exposure to these threats

---

## 🧱 Lab Environment

- 🧪 Cisco Packet Tracer  
- 💻 Hosts: PC-A, PC-B, Attacker Machine  
- 🌐 Networking gear: 2960 Switch, basic router  
- 💣 Tools simulated: switch flooding, crafted ARP responses, ICMP interception  

The attacker machine used scripting and flooding packets to disrupt network behavior and target communications.

---

## 🚨 Attack 1: MAC Flooding

### 🔹 Goal:
Overwhelm the switch’s CAM (Content Addressable Memory) table, forcing it into “fail-open” mode and flooding traffic out of all ports.

### 🔹 Steps:

1. Attacker flooded network with packets using **fake MAC addresses**
2. CAM table became saturated and couldn't associate MACs with ports
3. Switch began broadcasting frames to all ports — enabling **packet sniffing**
4. Used PC-A to observe mirrored traffic originally destined for PC-B

### 🔹 Outcome:
- Network performance degraded  
- Confidential packets were intercepted  
- Switch behavior emulated broadcast hub, violating segmentation intent

---

## ⚔️ Attack 2: ARP Spoofing (Poisoning)

### 🔹 Goal:
Redirect traffic by poisoning ARP cache entries, posing as gateway or victim device.

### 🔹 Steps:

1. Attacker sent forged ARP replies mapping victim IP to attacker’s MAC
2. PC-A and PC-B updated their ARP tables incorrectly
3. ICMP ping responses started routing via attacker’s device
4. Attacker captured payloads before forwarding to the intended destination (Man-in-the-Middle)

### 🔹 Observed Behavior:
- ARP tables showed incorrect MAC-to-IP bindings
- Pings between PC-A and PC-B were intercepted  
- Layer 3 redirection achieved with minimal effort

---

## 🧠 Attack Insights

- **MAC flooding** relies on exploiting hardware limitations — once overflowed, switches revert to hub-like behavior
- **ARP spoofing** abuses protocol trust — hosts blindly accept ARP replies without validation
- Combined, these attacks form the basis for **advanced MITM exploits**, **session hijacking**, and **data exfiltration**
- They illustrate why internal segmentation and trust validation are essential for modern networks

---

## 🔐 Countermeasures Implemented

| Defense Strategy           | Purpose                                |
|---------------------------|-----------------------------------------|
| Port Security             | Limits number of MACs per port          |
| Dynamic ARP Inspection    | Blocks spoofed ARP responses            |
| DHCP Snooping             | Validates source of IP assignments      |
| Static ARP Entries        | Prevents ARP poisoning on critical hosts|
| VLAN Segmentation         | Restricts broadcast domain scope        |

Each was configured and tested post-attack to verify mitigation effects.

---

## 📸 Screenshots

![image](/assets/MacFlooding-&-ARPspoofing/macflood.jpg)  

---
![image](/assets/MacFlooding-&-ARPspoofing/mitm.jpg)  

---
![image](/assets/MacFlooding-&-ARPspoofing/ecf-reverseshell.jpg)

---

## 📄 Full Report

📥 [Download the full MAC Flooding & ARP Spoofing lab documentation (PDF)](/MAC-Flooding-&-ARP-Spoofing/MAC-Flooding-&-ARP-Spoofing.pdf)

Includes attack scripts, configuration breakdowns, packet trace screenshots, CAM and ARP table analysis, and mitigation verification steps.

---

> _"The moment you trust your network without inspecting it is the moment you lose control."_  
> — Alvin Okiya

[![✔ BACK](https://img.shields.io/badge/BACK_TO_PORTFORLIO-Click_Here-red?logo=github)](/README.md)