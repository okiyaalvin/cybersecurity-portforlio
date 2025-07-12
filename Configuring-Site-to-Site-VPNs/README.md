# 🔐 Configuring Site-to-Site VPNs – Cisco IPsec Tunnel Lab

In this lab, I deployed and verified a site-to-site IPsec VPN between two remote networks using **Cisco Packet Tracer**. The goal was to enable secure communication over an untrusted backbone by applying real-world encryption and authentication policies at Layer 3. This wasn’t just a basic tunnel — it was a deep dive into **crypto maps**, **ISAKMP negotiation**, and the mechanics of how "interesting traffic" triggers secure channels between devices that shouldn’t trust the internet, but still need to talk across it.

---

## 📌 Objective

- Establish an encrypted VPN tunnel using IPsec between R1 and R3  
- Configure ISAKMP Phase 1 and IPsec Phase 2 settings using **manual ACLs and transform sets**  
- Activate crypto maps to identify and protect traffic across a public transport  
- Confirm encrypted communication through packet inspection and VPN tunnel status checks

---

## 🧱 Lab Topology Overview

| Device | Interface | IP Address       | Role                    |
|--------|-----------|------------------|-------------------------|
| R1     | G0/0      | 192.168.1.1      | LAN Gateway             |
| R3     | G0/0      | 192.168.3.1      | Remote LAN Gateway      |
| R1     | S0/0/0    | 10.1.1.2         | VPN Peer (to R3)        |
| R3     | S0/0/1    | 10.2.2.2         | VPN Peer (to R1)        |
| PC-A   | NIC       | 192.168.1.3      | Local LAN Host          |
| PC-C   | NIC       | 192.168.3.3      | Remote LAN Host         |

Untrusted backbone communication passes through a third router that’s **VPN unaware**, creating realistic Layer 3 separation between edge networks.

---

## 🔧 Phase 1: Prerequisites & Security Licensing

### 🚀 Activation of Securityk9 Package

Routers R1 and R3 required enabling of the `securityk9` module:
- Verified using `show version` command  
- Activated via `license boot module c1900 technology-package securityk9`  
- Confirmed successful boot with `securityk9` package using router reload

🔍 Key Insight: Without proper licensing, VPN features like ISAKMP or IPsec can’t be accessed — a step often skipped in simulations but crucial in production.

---

## 🛠️ Phase 2: IPsec VPN Configuration

### 🔹 Step 1 – ACL Definition (Interesting Traffic)

```bash
access-list 110 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255
```

This defines which traffic gets encrypted — traffic flowing between PC-A and PC-C.

### 🔹 Step 2 – ISAKMP Phase 1 Setup

```bash
crypto isakmp policy 10
 encryption aes
 authentication pre-share
 group 2
crypto isakmp key cisco address 10.2.2.2
```

Negotiates secure key exchange and sets pre-shared keys for tunnel authentication.

### 🔹 Step 3 – IPsec Phase 2 Setup

```bash
crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac
crypto map VPN-MAP 10 ipsec-isakmp
 set peer 10.2.2.2
 set transform-set VPN-SET
 match address 110
```

Sets encryption methods and links ACL to crypto map. Applied to outgoing interface:

```bash
interface s0/0/0
 crypto map VPN-MAP
```

Identical steps were mirrored on R3 with reverse peer IP (10.1.1.2) and matching transform set.

---

## 📊 Phase 3: VPN Verification & Tunnel Status

### 🔍 Before Traffic

```bash
show crypto ipsec sa
```

Result: No packets encrypted or decrypted — tunnel inactive.

### 🔍 After Traffic

Pinged PC-C from PC-A using:

```bash
ping 192.168.3.3
```

Rechecked:

```bash
show crypto ipsec sa
```

📈 Outcome: Packet counters incremented  
- Encapsulated: 3  
- Encrypt: 3  
- Decapsulated: 3  
- Decrypt: 3  

Confirmed the VPN tunnel dynamically responded to interesting traffic.

### 🚫 Uninteresting Traffic Test

Pinged PC-B (not in ACL scope):

```bash
ping 192.168.2.3
```

Result: Successful ping but no encryption observed — **VPN not triggered**, validating ACL accuracy.

---

## 🧠 Key Takeaways

- VPNs are triggered by **ACL-defined traffic** — understanding how to scope "interesting traffic" is critical  
- IPsec VPN configuration is **multi-phased** — each section builds trust and defines security expectations  
- Crypto maps must be applied to the correct outgoing interfaces — misplacement breaks tunnel functionality  
- Licensing and proper hardware capability affect real VPN deployment in live networks  
- Monitoring via `show crypto ipsec sa` provides operational insight into real-time encryption status

This lab mirrors scenarios faced by SOC teams and network admins tasked with ensuring **confidentiality across insecure links** — the kind of foundational skill no cybersecurity portfolio should be without.

---

## 🖼️ Screenshots
- isakmp policy

![ISAKMP Policy on R3](/assets/Configuring-S2S-VPNS/Isakmp-policy.jpg)  

---
- Unencrypted packets

![Unencrypted packets](/assets/Configuring-S2S-VPNS/Show-ipsec.jpg)  

---
- Encrypted Packets

![Encrypted Packet Counters](/assets/Configuring-S2S-VPNS/ipsec-sa-status.jpg)

---

## 📄 Full Report

📥 [Download the full Site-to-Site VPN Configuration lab (PDF)](/Configuring-Site-to-Site-VPNs/Configuring-Site-to-Site-VPNs.pdf)

Includes CLI logs, license outputs, ACL definitions, crypto map structures, and validation commands from PC-A and PC-C.

---

> _"Security isn't just encryption — it's knowing when, how, and **why** to encrypt."_  
> — Alvin Okiya