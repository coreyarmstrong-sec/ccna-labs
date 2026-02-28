# 📡 CCNA 200-301 — Packet Tracer Labs

**Cisco CCNA 200-301 | In Progress — Expected April 2026**  
All labs built in Cisco Packet Tracer. Each folder contains the `.pkt` file, configuration commands, and a written explanation of what was built and why.

---

## Purpose

These labs aren't just exam prep — every topology here connects to something I've implemented or am implementing in my actual homelab. The goal is to understand the *why* behind every command, not just memorize syntax.

---

## Lab Index

| # | Topic | Exam Domain | Status |
|---|---|---|---|
| 01 | Basic Device Setup & Navigation | 1.0 Network Fundamentals | ✅ Complete |
| 02 | IP Addressing & Subnetting | 1.6, 1.7 | ✅ Complete |
| 03 | VLAN Configuration & Trunking | 2.1, 2.2 | ✅ Complete |
| 04 | Inter-VLAN Routing (Router-on-a-Stick) | 2.1 | ✅ Complete |
| 05 | Spanning Tree Protocol (RSTP) | 2.5 | 🔄 In Progress |
| 06 | EtherChannel (LACP/PAgP) | 2.4 | 📋 Planned |
| 07 | Static Routing | 3.3 | 📋 Planned |
| 08 | OSPF Single Area | 3.4 | 📋 Planned |
| 09 | Standard & Extended ACLs | 5.6 | 📋 Planned |
| 10 | NAT & PAT | 4.1 | 📋 Planned |
| 11 | DHCP Configuration | 4.3 | 📋 Planned |
| 12 | SSH Hardening | 5.3, 5.4 | 📋 Planned |
| 13 | Layer 2 Security (DHCP Snooping, DAI, Port Security) | 5.7 | 📋 Planned |
| 14 | IPv6 Addressing & Routing | 1.8, 1.9 | 📋 Planned |
| 15 | HSRP / FHRP | 3.5 | 📋 Planned |

---

## Lab Format

Each lab folder contains:

```
labs/XX-topic-name/
├── README.md          ← Scenario, objectives, key commands, lessons learned
├── topology.pkt       ← Cisco Packet Tracer file
└── screenshots/       ← Verification output screenshots
```

---

## Homelab Connection

Where applicable, I note how each lab topic connects to my physical homelab:

- **VLANs** → pfSense VLAN segmentation (IoT, LAN, Servers, Management)
- **Firewall/ACLs** → pfSense firewall rules enforcing inter-VLAN policy
- **DHCP** → pfSense DHCP server managing all four VLAN segments
- **SSH Hardening** → Applied to all management interfaces in homelab
- **Routing** → pfSense routing between VLANs mirrors router-on-a-stick concepts

---

## Study Resources

- Jeremy's IT Lab (YouTube) — Primary video resource
- Cisco Packet Tracer — Lab environment
- CCNA 200-301 Exam Cram (6th Edition) — Reference
- subnettingpractice.com — Daily subnetting drills
- Boson ExSim — Practice exams (70%+ through material)

---

*Updated regularly as labs are completed. Each lab README documents what I built, what I broke, and what I learned.*
