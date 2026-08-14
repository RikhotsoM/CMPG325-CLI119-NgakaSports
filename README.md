# CMPG325-CLI119-NgakaSports
The CMPG325 Network Design Project (CLI-119) delivers a complete Packet Tracer implementation for the Ngaka Sports Development Foundation in Mahikeng. It features a 10.46.0.0/16 IP scheme, structured VLANs, and end-to-end default routing to the ISP. By reusing switches and resolving CR8 for shared printing, it balances budget and core performance.

**Project ID:** CMPG325-2026-119
**Client ID:** CLI-119
**Addressing Block:** 10.46.0.0/16
**Assigned Challenge:** Default Routing (Edge/ISP Path Design)


 **1.Reasonable Assumptions & Justifications**

**1.1 Organisation Structure & Departments**
**Assumption:** The foundation operates from one main office in Mahikeng with 5 core departments:
- Admin & Management
- Finance & HR
- Sports Programs & Coaching
- Facilities & Events
- IT Support
- Plus Guest Network for visitors
**Justification:** This structure logically fits a regional sports development organisation. The brief does not specify departments, so this is a reasonable functional layout.

**1.2 Users, Devices & Subnetting**
**Assumption:** 15–30 users per department; each department = 1 VLAN/subnet. One shared printer serving Finance and Sports Programs (the two departments that cannot print initially).
**Justification:** Scale matches the organisation type. Using /24 subnets within the assigned /16 block provides simplicity, manageability, and room for growth.

**1.3 Equipment & Budget Constraint**
**Assumption:** 1 Edge Router, 1 Core Router, **2 existing Layer 2 switches (reused)** — no new switches purchased.
**Justification:** Directly satisfies the constraint: "Limited equipment budget — reuse existing switches where possible."

**1.4 Change Request CR8 — Shared Printer**
**Assumption:** Finance (VLAN20) and Sports Programs (VLAN30) are on separate subnets and cannot print by default. Printer placed in Shared Printer VLAN 50. Routing will be configured to allow both departments access.
**Justification:** Directly addresses the requirement: "A shared printer zone must serve two departments that currently cannot print."

**1.5 Default Routing Design**
**Assumption:** Edge Router has default route → ISP. Core Router has default route → Edge Router.
**Justification:** Standard edge/ISP path design and fulfils the assigned networking challenge.

**2.Proposed Topology Overview**

                    ┌─────────────────────┐
                    │     ISP / INTERNET   │
                    │  Gateway: 10.46.254.1│
                    └──────────┬────────────┘
                               │
                    ┌──────────▼────────────┐
                    │    EDGE ROUTER        │
                    │  ← Default Route → ISP│
                    │  IP: 10.46.254.2      │
                    └──────────┬────────────┘
                               │
                    ┌──────────▼────────────┐
                    │    CORE ROUTER        │
                    │ ← Default Route → Edge│
                    │  IP: 10.46.254.6      │
                    └──────┬────────┬───────┘
                           │        │
              ┌────────────▼───┐  ┌──▼──────────────────────┐
              │   SWITCH 1    │  │       SWITCH 2           │
              │ (Reused)      │  │     (Reused)              │
              │               │  │                           │
              │ VLAN10  Admin │  │ VLAN20  Finance          │
              │ VLAN99  Guest │  │ VLAN30  Sports Programs  │
              │ VLAN100 IT    │  │ VLAN40  Facilities        │
              │               │  │ VLAN50  🖨️ SHARED PRINTER│
              └───────────────┘  └───────────────────────────┘


**3.IP Addressing Scheme**   

| VLAN | Department / Purpose    | Subnet           | Gateway         |
|------|-------------------------|------------------|-----------------|
| 10   | Admin & Management       | 10.46.10.0/24   | 10.46.10.1      |
| 20   | Finance & HR            | 10.46.20.0/24   | 10.46.20.1      |
| 30   | Sports Programs         | 10.46.30.0/24   | 10.46.30.1      |
| 40   | Facilities & Events      | 10.46.40.0/24   | 10.46.40.1      |
| 50   | Shared Printer Zone      | 10.46.50.0/24   | 10.46.50.1      |
| 99   | Guest Network            | 10.46.99.0/24   | 10.46.99.1      |
| 100  | IT Support              | 10.46.100.0/24  | 10.46.100.1     |
| —    | Edge ↔ ISP              | 10.46.254.0/30  | ISP: 10.46.254.1|
| —    | Edge ↔ Core             | 10.46.254.4/30  | Edge: 10.46.254.5|

**Printer IP:** 10.46.50.10/24
