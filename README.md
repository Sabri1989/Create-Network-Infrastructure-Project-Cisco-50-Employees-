# 🏢 Create Network Infrastructure Project – Cisco (50 Employees)

## 📌 Project Overview

This project presents a **complete, production-ready network design and implementation** for a medium-sized company with **50 employees** using **Cisco equipment**. The network is secure, redundant, scalable, and follows industry best practices.

| **Aspect** | **Details** |
|------------|-------------|
| **Company Size** | 50 employees |
| **Departments** | IT, Sales, HR, Management, Guest |
| **Core Technology** | Cisco Switching & Routing |
| **Redundancy** | HSRP + EtherChannel |
| **Security** | VLAN Isolation, ACLs, Port Security, SSH |
| **Monitoring** | SNMP, Syslog |

---

## 🎯 Key Features

- ✅ **Hierarchical Design** (Core, Distribution, Access layers)
- ✅ **6 VLANs** with complete isolation between departments
- ✅ **Inter-VLAN Routing** on Layer 3 Core Switches
- ✅ **HSRP (Hot Standby Router Protocol)** for gateway redundancy
- ✅ **EtherChannel** for link aggregation between switches
- ✅ **NAT & DHCP** on Edge Router for internet access
- ✅ **Port Security, BPDU Guard, DHCP Snooping**
- ✅ **Guest Network** fully isolated from internal resources
- ✅ **SSH** for secure remote management
- ✅ **SNMP & Syslog** for monitoring and logging

---

## 🧱 Network Topology

[INTERNET]
│
[ISP Modem/Router]
│
┌─────────┴─────────┐
│ Cisco ISR 4321 │ (Edge Router)
│ - NAT, ACL, DHCP │
└─────────┬─────────┘
│ (192.168.0.0/30)
┌─────────┴─────────┐
┌─────────┤ Core Switch L3 1 ├─────────┐
│ │ (Catalyst 9300) │ │
│ │ - HSRP Active │ │
│ └─────────┬─────────┘ │
│ │ │
│ ┌─────────┴─────────┐ │
│ │ Core Switch L3 2 ├─────────┘
│ │ (Catalyst 9300) │
│ │ - HSRP Standby │
│ └─────────┬─────────┘
│ │
┌─────┴─────┐ ┌─────┴─────┐
│ Access │ │ Access │
│ Switch 1 │ │ Switch 2 │
│ (2960-X) │ │ (2960-X) │
│ Floor 1 │ │ Floor 2 │
└───────────┘ └───────────┘

text

> 📐 **Full diagrams** are available in the `/diagrams` folder.

---

## 📊 VLAN & IP Addressing Plan

| VLAN ID | Name | Subnet | Gateway | Purpose |
|---------|------|--------|---------|---------|
| 10 | IT | 192.168.10.0/26 | 192.168.10.1 | IT Department |
| 20 | Sales | 192.168.20.0/26 | 192.168.20.1 | Sales Department |
| 30 | HR | 192.168.30.0/27 | 192.168.30.1 | HR & Admin |
| 40 | Guest | 192.168.40.0/27 | 192.168.40.1 | Guest Wi-Fi |
| 99 | Native | 192.168.99.0/30 | — | Trunk Native VLAN |
| 100 | Management | 192.168.100.0/28 | 192.168.100.1 | Network Device Management |

---

## 🖥️ Hardware Used

| Device | Model | Quantity | Role |
|--------|-------|----------|------|
| Core L3 Switch | Cisco Catalyst 9300-24S | 2 | Internal routing + HSRP |
| Edge Router | Cisco ISR 4321 | 1 | Internet gateway + NAT + Firewall |
| Access Switch L2 | Cisco Catalyst 2960-X | 2 | End-user connectivity |
| Access Point | Cisco AIR-AP1832I | 2 | Wireless (Employee + Guest) |
| Management Server | VM (Ubuntu) | 1 | Syslog + SNMP collector |

---

## ⚙️ Configuration Files

All configuration files are available in the [`/configs`](./configs) folder:

| File | Description |
|------|-------------|
| `core-switch-1-config.txt` | Active HSRP Core Switch |
| `core-switch-2-config.txt` | Standby HSRP Core Switch |
| `access-switch-1-config.txt` | Floor 1 Access Switch |
| `access-switch-2-config.txt` | Floor 2 Access Switch |
| `edge-router-config.txt` | Edge Router (NAT, DHCP, ACLs) |

### 🔧 Sample Configuration (Core Switch SVI with HSRP)

```cisco
interface vlan 10
 ip address 192.168.10.2 255.255.255.192
 standby 1 ip 192.168.10.1
 standby 1 priority 110
 standby 1 preempt
 no shutdown
🧪 Testing & Verification
Connectivity Test Results
Source	Destination	Expected	Result
PC in VLAN 10	Gateway (192.168.10.1)	Success	✅
PC in VLAN 10	PC in VLAN 20	Success	✅
PC in VLAN 10	8.8.8.8	Success	✅
Guest Wi-Fi	PC in VLAN 10	Fail	✅
Guest Wi-Fi	Internet	Success	✅
Useful Verification Commands
cisco
show vlan brief
show ip route
show standby brief
show etherchannel summary
show port-security
show ip nat translations
show ip access-lists
show dhcp binding
🚀 How to Deploy This Project
Prerequisites
Cisco devices (or Cisco Packet Tracer / GNS3 / Eve-NG)

Basic knowledge of Cisco IOS CLI

Console or SSH access to devices

Deployment Steps
Clone this repository

bash
git clone https://github.com/yourusername/corporate-network-cisco-50employees.git
Upload configuration files to respective devices via TFTP or copy-paste.

Verify connectivity using the verification commands above.

Update passwords and secrets for production use.

Connect to ISP and configure WAN interface according to your ISP's settings.

📁 Repository Structure
text
corporate-network-cisco-50employees/
├── README.md                       # This file
├── configs/
│   ├── core-switch-1-config.txt
│   ├── core-switch-2-config.txt
│   ├── access-switch-1-config.txt
│   ├── access-switch-2-config.txt
│   └── edge-router-config.txt
├── diagrams/
│   ├── logical-topology.png
│   ├── physical-topology.png
│   └── vlan-structure.png
├── documentation/
│   ├── implementation-guide.pdf
│   ├── testing-results.pdf
│   └── troubleshooting-guide.pdf
└── assets/
    └── corporate-network.pkt       # Packet Tracer file (if available)
🔐 Security Features Implemented
Feature	Purpose
Port Security	Prevent unauthorized devices on access ports
BPDU Guard	Prevent STP manipulation attacks
DHCP Snooping	Block rogue DHCP servers
Native VLAN isolation	Use VLAN 99 as unused Native VLAN
SSH instead of Telnet	Encrypted remote management
ACL on WAN interface	Block Guest network from internal access
Shutdown unused ports	Reduce attack surface
📈 Future Improvements
Add OSPF dynamic routing between Core Switches and Edge Router

Implement Cisco ISE for 802.1X authentication

Add NetFlow for traffic analysis

Deploy RADIUS/TACACS+ for centralized AAA

Implement QoS for VoIP traffic

Add second ISP link with failover using IP SLA

🤝 Contributing
Contributions, issues, and feature requests are welcome!
Feel free to check the issues page.

📝 License

👤 Author
SABRI MNAWWAR 

GitHub: @yourusername

LinkedIn: https://www.linkedin.com/in/sabri-mnawwar-017563278/

Email: mnawwarsabri@gmail.com

⭐ Thank You Very Much  ⭐ 

