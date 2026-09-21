# Milestone 3 — Final Evaluation
## Technical Report & Secure Network Implementation

**Student:** [Your Name]
**Project:** Multi-VLAN Network with Security Hardening
**Date:** September 2026

---

## 1. Executive Summary
This project implements a **secure, fully segmented local area network** using Virtual Local Area Networking (VLAN), 802.1Q trunking, and Router-on-a-Stick inter-VLAN routing. The design separates six distinct departments into independent broadcast domains — improving security, reducing broadcast traffic, and simplifying management. Comprehensive security controls have been applied: unused port lockdown, access port hardening, trunk protection, native VLAN security, and password encryption. All devices are configured, hardened, tested, and verified operational.

---

## 2. Network Topology
**Architecture:** 2-Tier Hierarchical / Extended Star
ISP-Router (1941)
↓
EDGE-Router (2911)
↓
CORE-Router (1941) — Inter-VLAN Gateway
↓ Gi0/0 (Straight-Through)
Switch-1 (Cisco 2960) — Access & Distribution — SECURED
↓ Gi0/2 (Crossover)
Switch-2 (Cisco 2960) — Access Layer — SECURED


---

## 3. VLAN & IP Addressing Scheme
| VLAN ID | VLAN Name | Subnet | Default Gateway | Assigned Ports | Security Purpose |
|---------|-----------|--------|-----------------|----------------|------------------|
| 10 | Admin_VLAN | 10.46.10.0/24 | 10.46.10.1 | Switch-1 Fa0/1 | Restricted management segment |
| 100 | IT_VLAN | 10.46.100.0/24 | 10.46.100.1 | Switch-1 Fa0/2 | Technical infrastructure isolation |
| 99 | Guest_VLAN | 10.46.99.0/24 | 10.46.99.1 | Switch-1 Fa0/3, Switch-2 Fa0/7 | Public/visitor traffic containment |
| 20 | Finance_VLAN | 10.46.20.0/24 | 10.46.20.1 | Switch-2 Fa0/5 | Sensitive financial data — highest protection |
| 30 | Sports_VLAN | 10.46.30.0/24 | 10.46.30.1 | Switch-2 Fa0/6 | Departmental segmentation |
| 50 | Printer_VLAN | 10.46.50.0/24 | 10.46.50.1 | Switch-2 Fa0/8 | Network appliance isolation |

---

## 4. Security Implementation — Hardening Applied

### 4.1 Unused Port Lockdown
**Why:** Prevents unauthorized devices from being plugged into open ports
- Switch-1: Fa0/4–Fa0/24 → `shutdown` — administratively disabled 
- Switch-2: Fa0/1–Fa0/4, Fa0/9–Fa0/24 → `shutdown` — administratively disabled 
- Only active, assigned ports remain enabled 

### 4.2 Access Port Hardening
**Why:** Prevents trunk spoofing — stops an attacker from converting an access port into a trunk
- All user ports configured: `switchport mode access` + `switchport nonegotiate`
- Ports permanently locked as access — cannot dynamically become trunk 
- VLAN assignment static — no dynamic negotiation 

### 4.3 Trunk Port Security
**Why:** Prevents VLAN hopping attacks
- Trunk mode set explicitly: `switchport mode trunk`
- Native VLAN standardized to VLAN 1 (consistent across all links)
- Allowed VLAN list restricted: **only required VLANs permitted** — no unnecessary traffic
- `switchport nonegotiate` applied — no DTP frames sent 

### 4.4 Password & Access Security
**Why:** Restricts administrative access to authorized personnel only
- `enable secret` — encrypted privileged-mode password on all devices 
- Console password — direct physical access protected
- VTY lines — remote access protected; `transport input ssh` enabled (Telnet disabled)
- `service password-encryption` — all passwords encrypted in running-config 

### 4.5 Router-on-a-Stick Gateway Protection
- Main interface Gi0/0: Layer 2 trunk — no IP assigned
- Each subinterface tied to specific VLAN via `encapsulation dot1Q <vlan-id>`
- Gateway IPs follow convention: `.1` = router, `.10+` = hosts 

---

## 5. End Device Configuration
| Device | Port | VLAN | IP Address | Subnet Mask | Gateway | Status |
|--------|------|------|-----------|-------------|---------|--------|
| Admin-PC | Sw1 Fa0/1 | 10 | 10.46.10.10 | 255.255.255.0 | 10.46.10.1 |  Operational |
| IT-PC | Sw1 Fa0/2 | 100 | 10.46.100.10 | 255.255.255.0 | 10.46.100.1 |  Operational |
| Guest-PC | Sw1 Fa0/3 | 99 | 10.46.99.10 | 255.255.255.0 | 10.46.99.1 |  Operational |
| Facilities-PC | Sw2 Fa0/7 | 99 | 10.46.99.11 | 255.255.255.0 | 10.46.99.1 |  Operational |
| Finance-PC | Sw2 Fa0/5 | 20 | 10.46.20.10 | 255.255.255.0 | 10.46.20.1 |  Operational |
| Sports-PC | Sw2 Fa0/6 | 30 | 10.46.30.10 | 255.255.255.0 | 10.46.30.1 |  Operational |
| Printer | Sw2 Fa0/8 | 50 | 10.46.50.10 | 255.255.255.0 | 10.46.50.1 |  Operational |

---

## 6. Verification & Testing Results

### 6.1 VLAN Verification
| Command | Expected Result | Status |
|---------|----------------|--------|
| `show vlan brief` (Sw1) | VLANs 10, 100, 99 present with correct ports |  Pass |
| `show vlan brief` (Sw2) | VLANs 20, 30, 99, 50 present with correct ports |  Pass |

### 6.2 Trunk Verification
| Command | Expected Result | Status |
|---------|----------------|--------|
| `show interfaces trunk` | Only permitted VLANs listed — no unauthorized traffic |  Pass |
| `show interfaces <port> switchport` | Administrative Mode = trunk / nonegotiate |  Pass |

### 6.3 Port Security Verification
| Command | Expected Result | Status |
|---------|----------------|--------|
| `show interfaces status` | Unused ports = disabled/shutdown |  Pass |
| `show running-config` | Passwords encrypted; plaintext passwords hidden |  Pass |

### 6.4 Connectivity Tests
| Source | Target IP | Result | Status |
|--------|-----------|--------|--------|
| Admin-PC | 10.46.10.1 | Reply |  Pass |
| IT-PC | 10.46.100.1 | Reply |  Pass |
| Guest-PC | 10.46.99.1 | Reply |  Pass |
| Finance-PC | 10.46.20.1 | Reply |  Pass |
| Sports-PC | 10.46.30.1 | Reply |  Pass |
| Facilities-PC | 10.46.99.1 | Reply |  Pass |
| Printer (via PC ping) | 10.46.50.10 | Reply |  Pass |

> **Note:** Initial single-packet loss observed on first ping is attributed to ARP resolution and trunk convergence — standard network behavior. All subsequent packets received at 0% loss.

---

## 7. Troubleshooting Log
| Issue | Root Cause | Resolution |
|-------|-----------|-----------|
| Initial ping timeouts | Ports assigned to incorrect VLAN | Corrected port-to-VLAN mapping  |
| Trunk inconsistency errors | Native VLAN mismatch between switches | Standardized Native VLAN to 1 on all trunks  |
| Ports showing `notconnect` | Cables not seated in correct ports | Verified physical connections; re-cabled to correct ports  |
| Ports showing red/shutdown | Security lockdown disabled all ports | Explicitly `no shutdown` on active ports only  |
| Command rejection | Typed outside configuration mode | Verified prompt context before entering commands  |

---

## 8. Evidence Inventory
| File | Description |
|------|-------------|
| `Milestone3_Network.pkt` | Final secured Packet Tracer project file |
| `screenshots/01-switch1-vlans.png` | VLAN assignments — Switch-1 |
| `screenshots/02-switch2-vlans.png` | VLAN assignments — Switch-2 |
| `screenshots/03-trunk-status.png` | Trunk ports & allowed VLAN list |
| `screenshots/04-core-gateways.png` | Core router subinterfaces & gateways |
| `screenshots/05-ping-admin.png` | Admin VLAN connectivity test |
| `screenshots/06-ping-it.png` | IT VLAN connectivity test |
| `screenshots/07-ping-guest.png` | Guest VLAN connectivity test |
| `screenshots/08-ping-finance.png` | Finance VLAN connectivity test |
| `screenshots/09-ping-sports.png` | Sports VLAN connectivity test |
| `screenshots/10-ping-printer.png` | Printer VLAN test |
| `screenshots/11-topology.png` | Full network overview — all links active |
| `screenshots/12-port-security.png` | Unused ports disabled — `show interfaces status` |
| `screenshots/13-trunk-hardening.png` | Trunk security — restricted VLAN list |
| `screenshots/14-password-encryption.png` | Passwords encrypted in configuration |

---

## 9. Deliverables Checklist
| Deliverable | Status |
|-------------|--------|
| Final Packet Tracer File (.pkt) |  Uploaded |
| GitHub Portfolio — Screenshots |  Complete |
| GitHub Portfolio — README Documentation |  Complete |
| Technical Report (this document) | Complete |
| Demonstration Video (15–20 min) | Pending |

---

## 10. Conclusion
This implementation demonstrates a **complete, secure, production-ready** multi-VLAN network with Router-on-a-Stick inter-VLAN routing. Through systematic design, configuration, hardening, and verification:

-  All VLANs created, named, and correctly assigned
-  802.1Q trunking established and secured against VLAN hopping
-  Unused ports disabled to prevent unauthorized access
-  Administrative access protected with encrypted passwords
-  End-to-end connectivity verified across all subnets
-  Network stable, segmented, and resilient

Key competencies demonstrated include VLAN management, trunk hardening, access port security, password policies, and structured troubleshooting. The network meets all functional and security requirements.

---

## 11. References
- Cisco Systems — Cisco IOS Command Reference: VLAN & Switching
- Cisco Systems — Switch Security Best Practices Guide
- CompTIA Network+ — VLAN Segmentation & Inter-VLAN Routing
- Cisco Packet Tracer 8.2 — Network Simulation Environment


