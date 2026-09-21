# Milestone 3 — Final Evaluation
## Technical Report & Portfolio of Evidence

**Student:** Rikhotso MP
**Project:** Multi-VLAN Network Implementation
**Date:** 16 October 2026


## 1. Executive Summary
This project implements a fully segmented local area network using Virtual Local Area Networking (VLAN) and 802.1Q trunking with Router-on-a-Stick inter-VLAN routing. The design separates six distinct departments into independent broadcast domains, enhancing security, reducing broadcast traffic, and enabling manageable network growth. All components have been configured, tested, and verified operational.


## 2. Design Methodology

### 2.1 Topology Selection
A **two-tier hierarchical architecture** was selected:
- **Core Layer:** 1941 series router providing inter-VLAN routing and gateway services
- **Access/Distribution Layer:** Two Cisco 2960 switches handling VLAN assignment and port-based segmentation
- **Upstream:** Connection to EDGE and ISP routers for simulated external connectivity

### 2.2 VLAN Rationale
| VLAN | Purpose | Benefit |
|------|---------|---------|
| 10 | Administration | Isolated management traffic |
| 100 | IT Department | Dedicated technical infrastructure |
| 99 | Guest & Facilities | Public/non-staff segmentation |
| 20 | Finance | Sensitive financial data — restricted access |
| 30 | Sports | Department-specific traffic |
| 50 | Printer Services | Network appliance isolation |

### 2.3 IP Addressing Strategy
- Base network: `10.46.0.0/16`
- Subnet mask: `255.255.255.0` (/24) per VLAN
- Gateway convention: First usable address (`.1`) assigned to router subinterface
- Host convention: Workstations assigned from `.10` onward



## 3. Device Configuration Summary

### 3.1 Switch-1
VLANs: 10, 100, 99, 20, 30, 50 created and named
Access Ports:
Fa0/1 → VLAN 10 (Admin)
Fa0/2 → VLAN 100 (IT)
Fa0/3 → VLAN 99 (Guest)
Trunk Ports:
Gi0/1 → Core Router — dot1Q, Native VLAN 1
Gi0/2 → Switch-2 — dot1Q, Native VLAN 1
All VLANs explicitly allowed on trunk links


### 3.2 Switch-2
VLANs synchronized with Switch-1
Access Ports:
Fa0/5 → VLAN 20 (Finance)
Fa0/6 → VLAN 30 (Sports)
Fa0/7 → VLAN 99 (Facilities)
Fa0/8 → VLAN 50 (Printer)
Trunk Port:
Gi0/1 → Switch-1 — dot1Q, Native VLAN 1


### 3.3 Core Router — Router-on-a-Stick
Main Interface Gi0/0: Layer 2 trunk — no IP address
Subinterfaces with 802.1Q encapsulation:
Gi0/0.10    → 10.46.10.1/24
Gi0/0.100   → 10.46.100.1/24
Gi0/0.99    → 10.46.99.1/24
Gi0/0.20    → 10.46.20.1/24
Gi0/0.30    → 10.46.30.1/24
Gi0/0.50    → 10.46.50.1/24
All subinterfaces: no shutdown


## 4. Verification Results
| Verification Method | Outcome |
|---------------------|---------|
| `show vlan brief` | All VLANs present with correct port assignments  |
| `show interfaces trunk` | Trunks active — all required VLANs forwarding  |
| `show ip interface brief` | All gateways UP/UP |
| Inter-VLAN ping tests | All subnets reachable  |
| Full topology inspection | All links green — no errors  |



## 5. Troubleshooting & Challenges
| Issue | Root Cause | Resolution |
|-------|-----------|-----------|
| Initial ping timeouts | VLAN99/100 misassigned — ports in default VLAN 1 | Corrected port-to-VLAN mapping on both switches  |
| Trunk inconsistency errors | Native VLAN mismatch between switches | Standardized Native VLAN to 1 on all trunk ports  |
| Deletion confusion | VLANs 99/100 temporarily removed | Restored as Guest/IT per design specification  |
| Console command rejection | Executed outside `(config)` mode | Confirmed prompt context before entering commands  |


## 6. Conclusion
This implementation demonstrates a complete working multi-VLAN network with inter-VLAN routing. Through systematic configuration, verification, and troubleshooting, all design requirements have been met. The network is stable, fully segmented, and production-ready. Key competencies demonstrated include VLAN management, 802.1Q trunking, Router-on-a-Stick configuration, and systematic network troubleshooting.


## 8. References
- Cisco Systems. Cisco IOS Command Reference — VLAN & Switching
- CompTIA Network+ — VLAN Segmentation & Inter-VLAN Routing
- Cisco Packet Tracer 8.2 — Simulation Environment
