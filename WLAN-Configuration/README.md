# 📡 WLAN Configuration – Cisco Packet Tracer Wireless Lab

This project involved configuring and securing wireless LAN environments in Cisco Packet Tracer, simulating both **home** and **enterprise-grade WLAN deployments**. It provided hands-on experience with router setups, Wireless LAN Controllers (WLC), VLAN segmentation, authentication systems, DHCP scopes, SNMP logging, and wireless client connectivity.

The lab was designed to reflect real-world network administration tasks, focusing on secure wireless design, logical planning, and access control.

---

## 📌 Objective

- Configure a wireless home router with SSID, DHCP, and basic encryption
- Deploy a controller-based enterprise WLAN with WPA2-Personal and WPA2-Enterprise security
- Integrate VLAN tagging, DHCP scope assignment, SNMP monitoring, and RADIUS authentication
- Connect multiple wireless clients across distinct networks and verify performance
- Simulate end-to-end connectivity between user devices and shared services

---

## 🏡 Section 1: Home Wireless Router Setup

### 🔹 DHCP & Addressing Configuration

- Assigned LAN IP: `192.168.6.1/27`
- Configured DHCP range to issue max **20 IP addresses**, starting from `.3`
- Changed Internet interface from dynamic IP to static as per addressing table
- Set DNS server manually to match lab design

### 🔹 SSID & Wireless Security

- Enabled 2.4GHz radio interface
- SSID: `HomeSSID`, using **WPA2-Personal**
- Passphrase: `Cisco123`, broadcast visibility enabled
- Channel: Set to standard channel 6 for optimal coverage

### 🔹 Router Hardening

- Updated default administrator credentials
- Enforced WPA2-Personal passphrase on wireless interface
- Reviewed DHCP allocations and verified SSID visibility across all clients

### 🔹 Client Connectivity

- Laptop, tablet, and smartphone connected wirelessly
- Clients obtained IP addresses correctly and could **ping each other** and access the internet
- DNS resolution verified, allowing web access via URL

---

## 🏢 Section 2: Wireless LAN Controller Deployment

### 🔹 VLAN Interfaces

| WLAN | VLAN ID | IP Address        | DHCP Gateway    |
|------|---------|-------------------|-----------------|
| WLAN 2 | 2     | `192.168.2.254`   | `192.168.2.1`   |
| WLAN 5 | 5     | `192.168.5.254`   | `192.168.5.1`   |
| Management | 100 | `192.168.100.254` | —               |

Interfaces were configured through the WLC web GUI and validated using `show interface brief`.

### 🔹 DHCP Scope Setup

- Scope Name: `Management`
- IP Range: `192.168.100.235 – 192.168.100.245`
- Network: `192.168.100.0/24`, Gateway: `192.168.100.1`
- DHCP service assigned to the wireless management segment

### 🔹 RADIUS Authentication Server

- Server IP: `10.6.0.254`, Port: `1812`
- Shared Secret: `RadiusPW`
- Integrated as authentication backend for **WLAN 5**

### 🔹 SNMP Logging Configuration

- SNMP Trap Receiver IP: `10.6.0.254`
- Community Name: `WLAN`, SNMP status enabled
- Logging verified via WLC monitoring tab

---

## 📶 WLAN Creation & Security Profiles

### 1️⃣ WLAN 2 – WPA2-PSK

- SSID: `SSID-2`  
- Security Mode: WPA2-Personal  
- Passphrase: `Cisco123`  
- FlexConnect Settings: Local Switching and Local Auth enabled

### 2️⃣ WLAN 5 – WPA2-Enterprise

- SSID: `SSID-5`  
- Security Mode: WPA2-Enterprise using 802.1X  
- Credentials: `userWLAN5` / `userW5pass`  
- RADIUS server enabled as AAA backend  
- FlexConnect Settings: Same as WLAN 2 for roaming capability

---

## 🧪 Wireless Client Provisioning

- Wireless Host 1: Connected to SSID-2 using WPA2-PSK
- Wireless Host 2:
  - Configured new profile
  - Connected to SSID-5 using PEAP authentication
  - Verified success through RADIUS login logs and browser access

Troubleshooting SSID visibility required **manual activation** of SSIDs in WLC settings before clients could detect the networks.

---

## ✅ End-to-End Connectivity Testing

- Wireless hosts successfully pinged each other  
- DNS resolution verified using simulated domain requests  
- Accessed external web server (`203.0.113.78`) via browser  
- RADIUS authentication logs confirmed proper Enterprise-grade login flow

---

## 🧠 Key Takeaways

- WPA2-Enterprise adds significant security for corporate WLANs through centralized identity management  
- SSID visibility, DHCP scopes, and VLAN tagging are critical configuration checkpoints  
- SNMP logging offers visibility for network health and security event tracking  
- RADIUS integration supports scalable and secure WLAN authentication  
- GUI-based WLCs simplify complex wireless setups — a crucial tool for network engineers and SOC teams

---

## 🖼️ Screenshots

![DHCP Scope Setup](/assets/WLAN-Configuration/dhcp-scope.png)  

---
![SSID Activation in WLC](/assets/WLAN-Configuration/ssid-act.png)  

---
![Wireless Host Connection](/assets/WLAN-Configuration/wireless-host.png)

---

## 📄 Full Report

📥 [Download the full WLAN Configuration lab documentation (PDF)](/WLAN-Configuration/WLAN-Configuration.pdf)

Includes step-by-step GUI walkthroughs, IP addressing tables, authentication logic, and troubleshooting notes for wireless deployment.

---

> _"Wireless networks aren't plug-and-play. They're stitched together with precision, policy, and protection."_  
> — Alvin Okiya