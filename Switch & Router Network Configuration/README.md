# 🧱 Switch & Router Network Configuration – Cisco Packet Tracer Lab

A practical simulation of how switches and routers work together to build secure, segmented enterprise networks. This lab focuses on VLAN topology, trunking, static routing, and Layer 2 security enhancements — all in a controlled Cisco Packet Tracer environment designed to mirror real-world deployments.

---

## 📌 Objective

- Design segmented VLAN architecture across two switches
- Configure trunk ports, native VLANs, and secure inter-switch links
- Implement router-on-a-stick for inter-VLAN communication
- Apply port-level security to mitigate rogue access and Layer 2 threats

---

## 🛠️ Lab Overview

- 🔧 Tools: Cisco Packet Tracer  
- 📐 Devices:
  - S1 and S2 – Cisco 2960 switches
  - R1 – Central router
  - PC-A and PC-B – End-user clients
- 🧬 Connectivity:
  - VLAN-based segmentation  
  - Trunk links between switches  
  - Subinterfaces on R1 for inter-VLAN routing

---

## 🔧 Configuration Highlights

### 🔹 VLAN Setup

- VLAN 10 – Workstations  
- VLAN 20 – Management  
- VLAN 30 – Finance  
- VLAN 99 – Native VLAN  
- VLAN 999 – Parking lot for unused ports  

Ports were statically assigned per VLAN, and all unused ports were disabled and moved to VLAN 999 to prevent rogue connections.

### 🔹 Trunking & Switch Hardening

- Enabled trunking with `switchport mode trunk`  
- Set VLAN 99 as native to prevent VLAN hopping  
- Disabled DTP negotiation using `switchport nonegotiate`  
- Protected trunk links with manual encapsulation and port restrictions  
- Configured sticky MAC addresses and limited access port connections using port security

### 🔹 Router-on-a-Stick Configuration

Subinterfaces were created on R1 for each VLAN:

```
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
```

Additional subinterfaces for VLANs 20 and 30 followed the same format, enabling IP routing between segments.

---

## ✅ Validation & Testing

- Pinged across VLANs to verify routing behavior  
- Observed port security in action using `show port-security`  
- Verified trunk links with `show interface trunk`  
- Confirmed MAC address learning and switch behaviors using `show mac address-table`  
- Monitored ARP tables and DHCP bindings to track endpoint communications

---

## 🧠 Key Takeaways

- VLANs provide isolation and control, reducing broadcast noise and improving security posture  
- Trunking, when paired with native VLAN reassignment and DTP disablement, prevents VLAN hopping  
- Router-on-a-stick is an efficient solution for inter-VLAN communication in small and medium setups  
- Port security and unused port lockdown are essential to prevent switch hijacking or unauthorized access  
- DHCP Snooping, BPDU Guard, and Layer 2 filtering add defensive depth to network architecture

---

## 🖼️ Screenshots

![Router-on-a-Stick Subinterfaces](/assets/S&N-Config/connection.jpg)  

---
![VLAN Trunk Verification](/assets/S&N-Config/verification.jpg)

---

## 📄 Full Report

📥 [Download the full Switch & Router Network lab documentation (PDF)](/Switch%20&%20Router%20Network%20Configuration/Switch%20&%20Router%20Network.pdf)

Includes complete configuration outputs, topology diagrams, command breakdowns, and explanations of every security feature applied.

---

> _"Layer 2 controls define boundaries. Layer 3 logic defines communication. Together, they shape a resilient network."_  
> — Alvin Okiya