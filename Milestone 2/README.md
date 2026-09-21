# Milestone 2 — Client Implementation Review

## Project Overview
This milestone covers the design, configuration, and verification of a multi-VLAN network infrastructure. The implementation uses **Router-on-a-Stick (ROAS)** inter-VLAN routing to provide secure, segmented communication across six distinct broadcast domains. All devices have been configured, tested, and verified operational.

## Network Topology
**Architecture:** 2-Tier Hierarchical / Extended Star
ISP-Router (1941)
↓
EDGE-Router (2911)
↓
CORE-Router (1941) — Inter-VLAN Gateway
↓ Gi0/0 (Straight-Through)
Switch-1 (Cisco 2960) — Distribution / Access Layer
↓ Gi0/2 (Crossover)
Switch-2 (Cisco 2960) — Access Layer



## VLAN & IP Addressing Scheme
| VLAN ID | VLAN Name | Subnet | Default Gateway | Assigned Ports |
|---------|-----------|--------|-----------------|----------------|
| 10 | Admin_VLAN | 10.46.10.0/24 | 10.46.10.1 | Switch-1 Fa0/1 |
| 100 | IT_VLAN | 10.46.100.0/24 | 10.46.100.1 | Switch-1 Fa0/2 |
| 99 | Guest_VLAN | 10.46.99.0/24 | 10.46.99.1 | Switch-1 Fa0/3, Switch-2 Fa0/7 |
| 20 | Finance_VLAN | 10.46.20.0/24 | 10.46.20.1 | Switch-2 Fa0/5 |
| 30 | Sports_VLAN | 10.46.30.0/24 | 10.46.30.1 | Switch-2 Fa0/6 |
| 40 | Facilities_VLAN | 10.46.40.0/24 | 10.46.40.1 | Switch-2 Fa0/7 |
| 50 | Printer_VLAN | 10.46.50.0/24 | 10.46.50.1 | Switch-2 Fa0/8 |



## Trunk Configuration
| Link | Port Pair | Encapsulation | Native VLAN | Allowed VLANs |
|------|-----------|---------------|-------------|---------------|
| Core ↔ Switch-1 | Gi0/0 ↔ Gi0/1 | 802.1Q | 1 | 1,10,20,30,50,99,100 |
| Switch-1 ↔ Switch-2 | Gi0/2 ↔ Gi0/1 | 802.1Q | 1 | 1,10,20,30,50,99,100 |



## End Device Configuration
| Device | IP Address | Subnet Mask | Gateway | Status |
|--------|-----------|-------------|---------|--------|
| Admin-PC | 10.46.10.10 | 255.255.255.0 | 10.46.10.1 |  Operational |
| IT-PC | 10.46.100.10 | 255.255.255.0 | 10.46.100.1 |  Operational |
| Guest-PC | 10.46.99.10 | 255.255.255.0 | 10.46.99.1 |  Operational |
| Facilities-PC | 10.46.99.11 | 255.255.255.0 | 10.46.99.1 | Operational |
| Finance-PC | 10.46.20.10 | 255.255.255.0 | 10.46.20.1 |  Operational |
| Sports-PC | 10.46.30.10 | 255.255.255.0 | 10.46.30.1 |  Operational |
| Printer | 10.46.50.10 | 255.255.255.0 | 10.46.50.1 |  Operational |



## Verification & Testing
All connectivity tests performed from device Command Prompt:

| Test | Target IP | Result | Status |
|------|-----------|--------|--------|
| Admin - Gateway | 10.46.10.1 | Reply |  Pass |
| IT - Gateway | 10.46.100.1 | Reply |  Pass |
| Guest - Gateway | 10.46.99.1 | Reply |  Pass |
| Finance - Gateway | 10.46.20.1 | Reply |  Pass |
| Sports - Gateway | 10.46.30.1 | Reply |  Pass |
| Facilities - Gateaway | 10.46.40.1 | Reply | Pass |
| Printer - Gateway | 10.46.50.1 | Reply |  Pass |


## Evidence Inventory
| File | Description |
|------|-------------|
| `Milestone2_Network.pkt` | Final Packet Tracer working file |
| `screenshots/01-switch1-vlans.png` | `show vlan brief and interface truck` — Switch-1 |
| `screenshots/02-switch2-vlans.png` | `show vlan brief and interface truck` — Switch-2 |
| `screenshots/04-core-gateways.png` | `show ip interface brief` — Core Router |
| `screenshots/05-ping-admin.png` | Admin VLAN connectivity test |
| `screenshots/06-ping-it.png` | IT VLAN connectivity test |
| `screenshots/07-ping-guest.png` | Guest VLAN connectivity test |
| `screenshots/08-ping-finance.png` | Finance VLAN connectivity test |
| `screenshots/09-ping-sports.png` | Sports VLAN connectivity test |
| `screenshots/10-topology.png` | Full network topology — all links active |

**Note:** Initial single-packet loss observed on first ping is attributed to ARP resolution and trunk convergence — standard network behavior. All subsequent packets received at 0% loss.

## Conclusion
Milestone 2 deliverables complete:
-  All VLANs created, named, and port-assigned
-  802.1Q trunking established between all devices
-  Router-on-a-Stick inter-VLAN routing configured and functional
-  End-to-end connectivity verified across all subnets
- Project file and evidence uploaded



