# 🛡️ VLANs and Secure Switch Configuration – Cisco Packet Tracer Lab

This project documents the secure setup and segmentation of a small enterprise network using VLANs, switch hardening, and Layer 2 security best practices. It simulates a multi-VLAN topology with Cisco Packet Tracer, implementing DHCP snooping, port security, BPDU Guard, and more to mitigate common switch-based attacks.

---

## 📌 Objective

- Design and configure multiple VLANs to enhance segmentation and network manageability
- Implement switch hardening via port security, DHCP snooping, trunk restrictions, and interface lockdown
- Simulate DHCP allocation, ARP activity, MAC learning, and inter-switch trunking
- Validate connectivity and switching behavior in a simulated environment

---

## 🛠️ Lab Environment

- 🖥️ **Cisco Packet Tracer**
- 🖧 Two 2960 switches (S1 and S2), a router (R1), and two PCs (PC-A, PC-B)
- 🎛️ Simulated interfaces, VLAN assignments, DHCP relay setup, and port security features

---

## 🔧 Configuration Highlights

### 🔹 VLAN & Trunking
- VLAN 10 – Management (SVI: `192.168.10.201` on S1 and `.202` on S2)
- VLAN 333 – Native VLAN for trunk link
- VLAN 999 – ParkingLot VLAN for unused ports
- Enabled **802.1Q trunking**, disabled **DTP negotiation**, and validated with `show interface trunk`

### 🔹 Switch Hardening
- **Port Security**: 
  - F0/6 on S1: Max 3 MACs, restrict mode
  - F0/18 on S2: Max 2 MACs, protect mode with sticky learning
- **DHCP Snooping**:
  - Enabled on VLAN 10
  - F0/1 marked trusted; F0/18 limited to 5 packets/sec
- **BPDU Guard & PortFast** enabled on access ports
- All unused ports:
  - Assigned to VLAN 999
  - Shutdown to prevent rogue device connection

---

## 🧪 Lab Verification

- Used `ping` from PC-A to PC-B to verify connectivity
- Verified `show port-security` and `show ip dhcp snooping` outputs
- Checked interface statuses, sticky MAC address learning, DHCP behavior, and VLAN routing

---

## 🧠 Key Takeaways

- VLANs are a core defense strategy for **layer 2 segmentation** and **broadcast domain isolation**
- Port security helps detect rogue devices by enforcing **MAC address control**
- DHCP snooping + trusted port designation protect against **rogue DHCP servers** and **starvation attacks**
- Trunk security (e.g. native VLAN assignment and DTP disablement) is vital for preventing **VLAN hopping**
- BPDU Guard and PortFast reduce the risk of **spanning tree loops** from accidental switch connection

---

## 🖼️ Screenshots

_Add screenshots like the ones below once available:_

```markdown
![VLAN Trunk Config](./vlan-trunk-verify.png)
![DHCP Snooping Status](./dhcp-snoop-show.png)
```

---

## 📄 Full Report

📥 [Download the full VLAN and Switch Security lab documentation (PDF)](./VLAN-Switch-Config.pdf)

This report includes the complete configuration steps, command outputs, topology screenshots, MAC learning behaviors, and explanation of every applied security measure.

---

> _"A secure switch is not just a configuration — it's a promise to never let traffic go where it shouldn't."_  
> — Alvin Okiya